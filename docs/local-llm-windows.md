# Run the food-estimating AI on your Windows PC

This sets up a local language model (via [Ollama](https://ollama.com)) on your Windows PC and lets the Macro Tracker app reach it **privately through Tailscale**. The app then turns text like "2 eggs, a slice of toast and a glass of ayran" into calories, protein, carbs and fat.

Nothing is exposed to the public internet. Only devices signed in to your own Tailscale network can reach the PC.

**You need:** an NVIDIA GPU with 8 GB of VRAM (your 3070 Ti is fine), Windows 10/11, and Tailscale already installed and signed in on the PC.

---

## 1. Install Ollama and the model

1. Download and run the Windows installer from <https://ollama.com/download/windows>.
2. Open **PowerShell** and download the model (about 4.7 GB, one time):

   ```powershell
   ollama pull qwen2.5:7b-instruct
   ```

3. Try it:

   ```powershell
   ollama run qwen2.5:7b-instruct "How much protein is in 2 boiled eggs? Answer in one line."
   ```

4. Confirm it runs on the GPU, not the CPU. While the model is loaded, run:

   ```powershell
   ollama ps
   ```

   The `PROCESSOR` column should say `100% GPU`. If it says CPU, update your NVIDIA driver and try again.

## 2. Allow the web app to talk to Ollama

Browsers block a website from calling another server unless that server allows it. Tell Ollama to allow your app's address and to keep the model loaded between requests:

```powershell
setx OLLAMA_ORIGINS "https://mrtozdgn.github.io"
setx OLLAMA_KEEP_ALIVE "30m"
```

Then **restart Ollama**: right-click the Ollama icon in the system tray, choose **Quit**, and open Ollama again from the Start menu. (`setx` only affects programs started afterwards.)

## 3. Publish it privately with Tailscale Serve

1. In the Tailscale admin console ([login.tailscale.com/admin/dns](https://login.tailscale.com/admin/dns)) make sure **MagicDNS** and **HTTPS Certificates** are enabled.
2. In PowerShell on the PC:

   ```powershell
   tailscale serve --bg 11434
   ```

3. Show the address it created:

   ```powershell
   tailscale serve status
   ```

   It looks like `https://your-pc.your-tailnet.ts.net`. That is your **server address**.

> If `tailscale serve --bg 11434` isn't recognised, update Tailscale to the latest version.
>
> **Do not use `tailscale funnel`** for this. Funnel makes the address public, and Ollama has no password.

## 4. Check it from your phone or laptop

With Tailscale **connected** on that device, open this in a browser (use your own address):

```
https://your-pc.your-tailnet.ts.net/api/tags
```

You should see JSON that lists `qwen2.5:7b-instruct`. If the page doesn't load, Tailscale probably isn't connected on that device.

## 5. Connect the app

1. Open the app, tap the gear icon, and scroll to **Local AI (Ollama)**.
2. Paste the **server address** and set the model to `qwen2.5:7b-instruct`.
3. Press **Test connection**. It should say "Connected" and confirm the model is installed.
4. Press **Save**.

Now in **Meals → Add food**, type what you ate in the **Describe what you ate** box and press **Estimate with AI**. Check the numbers, then **Add all**, or tap **Edit** on any item to adjust it first. The server address stays on your device and is never included in backups or GitHub sync.

---

## Keeping the PC always ready

- **Never sleep while plugged in:**

  ```powershell
  powercfg /change standby-timeout-ac 0
  powercfg /change hibernate-timeout-ac 0
  ```

  (The monitor can still turn off: `powercfg /change monitor-timeout-ac 10`.)
- **After a reboot,** Ollama starts when you sign in to Windows. If this PC should recover from a power cut or an update restart without you, enable automatic sign-in (Settings → Accounts → Sign-in options) or run Ollama as a service.
- **Windows Update restarts:** set Active hours so updates restart at a time you're asleep.
- The first request after a long idle takes a few seconds while the model loads into the GPU. Later ones take about 1 to 3 seconds.

## Troubleshooting

| Symptom in the app | Likely cause and fix |
|---|---|
| "Couldn't reach your AI server" | Tailscale isn't connected on that device, or the address in Settings is wrong. Test the `/api/tags` address in a browser. |
| Works in a browser tab but the app fails | `OLLAMA_ORIGINS` isn't applied. Re-run the `setx` line, then fully quit and reopen Ollama. |
| "Model not found" | The model name in Settings must match `ollama list` exactly. |
| "The AI took too long" | PC asleep, or the model is loading. Try again. Check `ollama ps` shows it on the GPU. |
| Numbers look wrong | It is a small model estimating from general knowledge. Tap **Edit**, or log a food from search for precise values. |

## Choosing a different model

Any instruction-tuned model that fits in 8 GB works. Qwen2.5 7B handles Turkish well. To try another, `ollama pull` it, put its exact name in Settings, and press Test connection. For example `llama3.1:8b` or `qwen3:8b`.
