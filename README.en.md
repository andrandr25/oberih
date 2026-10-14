# Anonymization module Oberih. Installation and Setup Guide

Українська версія: [README.md](README.md)

| Item | Value |
|---|---|
| Device | GL.iNet GL-E750 (Mudi V2) |
| Device firmware | GL 4.8.5 (recommended); GL 4.3 supported |
| Software | Oberih 2.6 and later, channel "Stable". Before version 2.7 the router menu calls it **Mudi Core** |
| Package in the plug-in list | `luci-app-mudi` |
| Document edition | 1.1, 2026-10-14 |

This document describes how to install Oberih on a GL-E750 router and set up a protected connection. Every technical term is explained in plain words where it first appears. The document is updated with each Oberih release; see section 10 for the change history.

---

## 1. What Oberih is

The **anonymization module Oberih** (Ukrainian: «Оберіг», "a protective charm") is installed as a plug-in (in plain words: an add-on program) for the admin panel of the GL-E750 router. It is built into the standard GL panel and does three things:

1. **Protected connection through a VPN** (in plain words: all internet traffic of your devices goes through an encrypted channel to a VPN server, so neither your mobile carrier nor the network you are connected to can see which sites you open).
2. **VPN failover** (in plain words: if one VPN server stops working, the router moves to another one by itself).
3. **Protection checklist** (in plain words: the router shows what is set up correctly and what is not, and fixes it with one button).

Oberih makes every setting through GL's own mechanisms, so the GL panel shows the same values as Oberih.

---

## 2. Requirements

| What you need | Note |
|---|---|
| GL-E750 router | Charged or on power |
| Internet access for the router | A SIM card with mobile data, or a Wi-Fi network the router joins as a client |
| A phone or computer with a browser | To open the admin panel |
| Router admin password | Set at first login (step 4.1) |
| VPN account (optional) | A Mullvad account number, or a WireGuard configuration file from your VPN provider. Without one you can use the free Cloudflare WARP |

---

## 3. Terms

| Term | In plain words |
|---|---|
| Admin panel | The router's settings page, opened in a browser at `192.168.8.1` |
| Firmware | The router's main program from GL.iNet |
| Plug-in | An additional program installed on top of the firmware |
| Plug-in source | An internet address the router downloads the plug-in from |
| Update channel | Where the router gets new Oberih versions. "Stable": tested versions for everyday use |
| VPN | An encrypted channel between the router and a VPN server; hides your traffic from the carrier and other networks |
| WireGuard | The modern VPN protocol the router uses |
| VPN node | One VPN server the router can connect to |
| Failover | Moving to another node automatically when the current one does not answer |
| Kill switch | Blocking internet for your devices while the VPN is not working, so traffic never goes around the VPN |
| Cloudflare WARP | A free VPN by Cloudflare; the router registers its own account automatically |
| SSID | The name of the router's Wi-Fi network |
| Device ID | The router's identifier; printed on the sticker underneath, shown in the GL panel and in Oberih. Needed when you contact support |

---

## 4. Installation

### 4.1. First login to the admin panel

1. Turn the router on. Wait 1–2 minutes for it to start.
2. Join the router's Wi-Fi. The network name and password are printed on the sticker underneath (the name looks like `GL-E750-xxx`).
3. Open `192.168.8.1` in a browser.
4. Choose a language and set an admin password.

In plain words: the admin password protects the router's settings. Write it down; without it the settings cannot be changed.

### 4.2. Checking the firmware version

1. In the panel open **SYSTEM → Upgrade**.
2. Check the current version. The recommended version is **4.8.5**.
3. If it is older, upgrade in the same section (online, or with a file from the official GL.iNet site).

Do not turn the router off during a firmware upgrade. It takes 3–5 minutes; the router then restarts and you need to join its Wi-Fi again.

In plain words: Oberih is tested on 4.8.5. On other versions it works, but any feature whose compatibility is not confirmed is switched off automatically, with the reason shown.

### 4.3. Connecting the router to the internet

The router needs its own internet access to download Oberih.

- **SIM card:** insert a SIM card with mobile data. The router connects automatically.
- **Wi-Fi (Repeater):** in **INTERNET**, connect the router to an existing Wi-Fi network.

### 4.4. Adding the Oberih source

1. Open **APPLICATIONS → Plug-ins**.
2. Click **Manage Sources**, then **Add**.
3. Fill in:

| Field | Value |
|---|---|
| Name | `mudi` |
| URL | `https://git.fraola.net/androffka/mudi-updates/raw/branch/main/stable` |

4. Save.

In plain words: you tell the router where to get Oberih. Every Oberih version carries a digital signature; the router installs only versions with a valid signature.

### 4.5. Installing Oberih

1. On the **Plug-ins** page click **Update** so the router downloads the list of plug-ins. Wait up to 30 seconds.
2. Search for `luci-app-mudi`.
3. Click **Install**. Installation takes up to 1 minute.
4. Reload the browser page.

**Oberih** appears in the menu (in versions before 2.7: **Mudi Core**). The setup wizard starts when you open it for the first time.

---

## 5. Setup wizard

The wizard has five steps. **Apply and continue** saves a step and goes to the next one. **Later** closes the wizard; it opens again until it is finished.

### 5.1. Step "VPN"

Choose one or more ways to connect.

| Way | What to do | In plain words |
|---|---|---|
| Cloudflare WARP | **Use WARP**, accept Cloudflare's terms | A free VPN. The router creates its own account. Good as the main or the backup connection |
| Mullvad account | Enter the account number → **Connect** → **Choose countries** → pick up to 4 countries → **Add to VPN** | Paid Mullvad VPN with your account number. The router takes one device slot in your account |
| WireGuard file | **Import a WireGuard .conf** → **Choose file**, or **Paste text** | A configuration from any VPN provider (Proton, Mullvad, others) |

Important for a Mullvad account: connect the router to the account **once**. Connecting again replaces all servers chosen before.

After choosing:

- **Node order.** The first node in the list is the main one, the rest are backups. Change the order with the arrows. Recommended: your paid VPN first, Cloudflare WARP last.
- **Kill switch:** leave on.
- **Failover:** leave on (available with two or more nodes).

### 5.2. Step "Protection"

The wizard shows protection settings worth turning on (for example forced DNS through the router, DNS rebinding protection, IPv6 off). Keep the suggested choice.

In plain words: these settings stop devices and websites from going around the VPN or finding out your real network.

### 5.3. Step "Privacy"

Offers to install **Blue Merle**, a module that keeps the history of connected devices in memory only and lets you change the modem's IMEI with the switch on the side of the router.

A notice about changing the IMEI is shown before installation. Whether changing an IMEI is legal depends on the country you are in. If you are not sure, click **Skip**.

### 5.4. Step "Wi-Fi"

The wizard suggests a new network name and password.

- **New random:** generate other values.
- **Keep current:** keep the name and password from the sticker.

After saving, the router restarts its Wi-Fi and your phone or computer disconnects. **Join the network with the new name and password** shown by the wizard. Write them down.

### 5.5. Step "Done"

Setup is complete. Oberih opens its main page with the protection status.

---

## 6. Everyday use

After setup the router needs nothing from you. Below are features you may need.

### 6.1. Protection status

The top of the Oberih page shows a protection score and five indicators: identity, VPN, DNS, leak test, Wi-Fi range. **Fix All Issues** applies the recommended settings.

**Run Leak Test** (from version 2.7) checks not only the router itself but what a device on your network sees: for a few seconds the router creates a temporary virtual device and checks which IP address it reaches the internet from.

| Result | In plain words |
|---|---|
| No leaks | Devices reach the internet only through the VPN |
| Leak | Devices go out directly with your real IP address. Check that the VPN and the kill switch are on |
| Devices offline | The VPN is not working and the block does not let traffic past the VPN. Check the VPN nodes |

### 6.2. VPN failover

Section **VPN Multi-node & Failover**.

| Feature | In plain words |
|---|---|
| Automatic switching | If the current node does not answer, the router moves to the next one that does. When the mobile link drops, nodes are not switched; the router waits for the link to come back |
| **VPN always on** | If the VPN goes off, the router turns it back on within seconds |
| **Block internet past the VPN** (from version 2.6.14) | Your devices reach the internet only through the VPN, even while it reconnects. If no VPN node works, devices have no internet |
| **Connect** | Move to the chosen node by hand. The router will not return to the main node for at least 10 minutes |
| **Add Node** | Add another VPN server: WARP, a Mullvad account or a profile from the GL panel |

### 6.3. VPN history

The **VPN Multi-node & Failover** section (from version 2.7) lists recent events in plain words, for example "Austria did not answer: switched to Belgium", "Mobile link lost: VPN nodes kept as they were". Times are the router's clock.

### 6.4. Networks with a login page (hotel, airport)

Some Wi-Fi networks require a login through a browser page. While the VPN is on, that page does not open.

1. Click **Stop VPN** and confirm. In plain words: traffic temporarily goes directly, without the VPN.
2. Open the network's login page and log in.
3. In **VPN Multi-node & Failover**, click **Connect** on the node you want.

After **Stop VPN** the router does not turn the VPN on by itself until you connect a node.

### 6.5. Updating Oberih

Section **Updates**.

| Setting | Recommended value | In plain words |
|---|---|---|
| Channel | **Stable** | Tested versions only |
| Install updates automatically | On | Once a day the router checks for updates and installs them at the chosen hour |
| Install at (hour) | 4 | During installation the VPN may be unavailable for up to 2 minutes |

Every update carries a digital signature that is checked before installation. If a new version does not start, the router restores the previous one automatically.

---

## 7. Troubleshooting

| Symptom | Possible cause | Action |
|---|---|---|
| No connection to the router after the "Wi-Fi" step | The Wi-Fi name and password were changed | Join the network with the new name |
| Oberih is missing from the menu | The page was not reloaded after installation, or a version before 2.7 is installed | Reload the browser page; before 2.7 the item is called **Mudi Core**; on GL 4.3 the item is at the bottom of the menu |
| `luci-app-mudi` is not found | The plug-in list was not refreshed, or the router has no internet | Check the router's internet (step 4.3), click **Update** on the plug-ins page |
| Devices have no internet, the VPN does not connect | **Block internet past the VPN** is on and no node answers | Check the router's internet; click **Check All Nodes**; if needed, **Stop VPN** |
| A hotel login page does not open | The VPN blocks it | Section 6.4 |
| The VPN keeps switching between nodes | Unstable mobile link or an overloaded server | Change the node order; put the node with the lowest latency first |

---

## 8. Contacting support

This guide also opens from the router: **Support** → **Guide** (from version 2.7).

Before contacting support, prepare:

1. **Device ID**: on the Oberih page, section **Support**, or on the sticker underneath the router.
2. A description: what happens, since when, after which actions.
3. Diagnostics: **Support** → **Copy diagnostics** (from version 2.7), paste the text into your message.
4. If possible, a screenshot of the Oberih page.

Do not send VPN account numbers, passwords or configuration files to support.

---

## 9. Data security

- Oberih does not send your traffic, passwords or VPN account number to support or to the update server.
- When checking for updates, the router tells the update server the Oberih version, the model, the firmware version and the Device ID, so support can see which routers have updated.
- VPN keys are stored on the router only.

---

## 10. Document changes

| Edition | Date | Oberih version | Changes |
|---|---|---|---|
| 1.1 | 2026-10-14 | 2.6, 2.7 | Name: anonymization module Oberih (formerly Mudi Core), with notes for versions before 2.7. Leak test from the devices' side, VPN history, diagnostics for support, link to the guide. English edition |
| 1.0 | 2026-10-06 | 2.6 | First edition (Ukrainian) |
