# Sysmon Investigation — THM-PC Initial Findings

## Host and user

- Host: `THM-PC`.
- User shown in supplied Sysmon evidence: `THM-PC\sarah.miller`.
- Browser: Google Chrome.

## Event types shown

`1` process creation; `3` network connection; `10` process access; `11` file creation; `13` registry value set; `15` file stream created; `22` DNS query.

## Reported download and artifact

- Executable path: `C:\Users\sarah.miller\Downloads\ckjg.exe`.
- URL in notes: `http://gettsveriff.com/bgj3/ckjg.exe`.
- Startup-folder artifact: `C:\Users\sarah.miller\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\DeleteApp.url`.

The Event ID `11` screenshot shows a Startup-folder path and process image `ckjg.exe`. This supports investigating a possible persistence mechanism; inspect the `.url` contents as text in an isolated environment before concluding what it does or whether persistence succeeded.

## Network/DNS indicators

- Destination recorded: `193.46.217.4:7777`.
- DNS name recorded: `hkfasfsafg.click`.

Treat these as training-scenario indicators, not independently verified live C2 infrastructure. Confirm the process association, timestamps, and DNS result in full records.

## Next steps

Correlate process IDs/GUIDs and timestamps; inspect full command line, parent process, hashes and signature; inspect the `.url` as text without opening its target; review Event IDs `3` and `22`; preserve relevant records. Do not execute the suspicious file or browse to the URL.
