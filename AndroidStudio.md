# Android Studio
### Hyper off
Da Android Studio allerede bruger en hyper emulgator skal man måske slå hyper fra. Det kan du gøre sådan:
```
bcdedit /set hypervisorlaunchtype off
```
Man slår dette fra:
❌ Hyper-V bliver deaktiveret (ved boot)
❌ Windows Hypervisor Platform stopper med at virke
❌ Virtual Machine Platform stopper også

Når Hyper-V er væk:

✔ Android Emulator kan bruge:
→ Android Emulator Hypervisor Driver

NB: **Slå software til under Graphic Accelerator for at kunne kunne køre emulatoren, hvis du ike har dedikeret kort til det.**
