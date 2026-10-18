# Automated VPN Deployment for Cloud PCs

A pilot project to deliver a VPN client to Windows 365 Cloud PCs automatically through Intune, with a device-specific background configuration — so remote endpoints route through a controlled, known egress without manual setup.

This repo documents the approach, the problems I hit, and how I validated the result. Environment-specific details (hostnames, addresses, tenant specifics) are intentionally kept generic.

## What I set out to do
- Stand up a cloud-hosted OpenVPN server as a pilot.
- Package and deploy the VPN client to Cloud PCs via Intune.
- Apply a device-specific background configuration so the tunnel comes up without user intervention.
- Verify connectivity, egress, and sign-in behavior end to end.

## Architecture (high level)
```
Windows 365 Cloud PC  ──Intune deployment──>  VPN client + config
         │
         └──encrypted tunnel──>  Cloud-hosted VPN server  ──>  Controlled egress
```

## What I ran into, and how I solved it
- **Install failures on the test device.** Traced the root cause to an incorrect Windows service name referenced in the deployment script. Corrected it; Intune then reported a clean install on the test device with zero failures or pending installs.
- **Connectivity validation.** Confirmed the tunnel came up after reboot and verified the expected VPN egress address in fresh identity-provider sign-in activity.
- **Sign-in dependency.** Identified a sign-in dependency that affected Cloud PC entry, and documented the policy-scope change needed to resolve it.

## Results
- ✅ Working OpenVPN pilot, server-side.
- ✅ Clean Intune test-device install after the script fix.
- ✅ Verified post-reboot connectivity and expected egress.
- ⏳ Broader rollout and full security validation in progress.

> **Status:** Working pilot. Rollout and security hardening are ongoing. Scripts and configuration will be published here with sensitive values removed.

## Repo structure (planned)
```
/scripts      - deployment and detection scripts (sanitized)
/intune       - packaging and configuration notes
/docs         - validation steps and troubleshooting write-ups
```

---
Part of my IT portfolio — see my [profile](https://github.com/kkmartins).
