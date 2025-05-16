# Install waydroid

Waydroid এর মধ্যমে আপনি whatsapp সহ এরও অনেক App ব্যবহার করতে পারবেন :

waydroid ইন্সটল করার একটা সহজ গাইড ->

1. Wayland কম্পোজিটর চলছে কিনা তা পরীক্ষা করা
Waydroid Wayland-এর জন্য ডিজাইন করা হয়েছে। আপনার সিস্টেমে Wayland সেশন চলছে কিনা তা পরীক্ষা করতে এই কমান্ডটি চালান:
```bash
echo $XDG_SESSION_TYPE
```

যদি আউটপুট wayland হয়, তাহলে আপনার Wayland সেশন চলছে। যদি x11 বা অন্য কিছু হয়, তাহলে Wayland সেশন শুরু করার জন্য আপনার লগইন স্ক্রিনে Wayland অপশনটি নির্বাচন করতে হতে পারে অথবা আপনার ডেস্কটপ এনভায়রনমেন্ট Wayland সমর্থন করে কিনা তা নিশ্চিত করুন।

2. binder_linux মডিউল </br>
Waydroid ব্যবহার করতে আপনার binder_linux মডিউল লাগবে যা linux-cachyos অথবা linux-zen এ আগে থেকেই থাকে যাতে আপনি সহজে Android apps ব্যবহার করতে পারবেন | আপনার বর্তমান কার্নেল পরীক্ষা করতে এই কমান্ডটি ব্যবহার করুন:
```bash
uname -r
```
এই কমান্ড আপনার চলমান কার্নেলের সংস্করণ দেখাবে। আউটপুটে কার্নেলের নামটি দেখতে পাবেন। যদি আপনার linux-cachyos বা linux-zen ইনস্টল করা না থাকে এবং আপনি সেগুলো ব্যবহার করতে চান, তাহলে আপনি আপনার প্যাকেজ ম্যানেজার ব্যবহার করে ইনস্টল করতে পারেন। উদাহরণস্বরূপ:
```bash
sudo pacman -S linux-cachyos linux-cachyos-headers
  # অথবা
sudo pacman -S linux-zen linux-zen-headers
```
ইনস্টল করার পরে, আপনাকে সিস্টেমে রিস্টার্ট করতে হবে এবং GRUB বা বুটলোডারের মেনু থেকে নতুন কার্নেলটি নির্বাচন করতে হবে।

3. AUR থেকে Waydroid এবং GApps ইমেজ ইনস্টল করা

Waydroid এবং GApps (Google Apps) ইমেজ ইনস্টল করার জন্য এই কমান্ডগুলো চালান:
```bash
yay -S waydroid
yay -S waydroid-image-gapps
```
এই কমান্ডগুলো Waydroid প্যাকেজ এবং Google Play Store সহ Waydroid ইমেজ ডাউনলোড, কম্পাইল এবং ইনস্টল করবে। প্রক্রিয়াটি আপনার সিস্টেমের গতি এবং নির্ভরতাগুলোর উপর নির্ভর করে কিছুটা সময় নিতে পারে।

4. প্রাথমিক সেটআপ এবং Waydroid চালানো

ইনস্টলেশন এবং ফায়ারওয়াল কনফিগারেশন সম্পন্ন হওয়ার পর, আপনাকে Waydroid ইনিশিয়ালাইজ করতে হবে এবং কন্টেইনার পরিষেবা শুরু করতে হবে।

	* Waydroid ইনিশিয়ালাইজ করুন। ইনস্টল করা GApps ইমেজ স্বয়ংক্রিয়ভাবে সনাক্ত হওয়া উচিত	:
```bash
sudo waydroid init
```
	* systemd এর মাধ্যমে waydroid চালু করা :
```bash
sudo systemctl enable --now waydroid-container
```
এরপর আপনার অ্যাপ্লিকেশন মেনু থেকে Waydroid চালু করুন।

এখন আপনার আর্চ লিনাক্স সিস্টেমে Waydroid ইনস্টল এবং চালু হওয়া উচিত এবং আপনি অ্যান্ড্রয়েড অ্যাপস ব্যবহার শুরু করতে পারবেন। GApps ইমেজ ইনস্টল করার কারণে আপনার Google Play Store উপলব্ধ থাকা উচিত।

5. ফায়ারওয়াল কনফিগারেশন

Waydroid ইন্টারনেট ব্যবহার করার জন্য আপনার ফায়ারওয়াল সঠিকভাবে কনফিগার করা প্রয়োজন। আপনার সিস্টেমে UFW (ufw) বা Firewalld (firewalld) কোনটি ব্যবহার করছেন তার উপর নির্ভর করে কনফিগারেশন ভিন্ন হবে।

আপনার ব্যবহৃত ফায়ারওয়ালের জন্য প্রযোজ্য নির্দেশাবলী অনুসরণ করুন:

যদি আপনি UFW ব্যবহার করেন:

Waydroid একটি নেটওয়ার্ক ইন্টারফেস তৈরি করে (সাধারণত waydroid0) যার মাধ্যমে এটি হোস্ট সিস্টেমের নেটওয়ার্ক ব্যবহার করে। এই ইন্টারফেসের মাধ্যমে ট্র্যাফিক অনুমোদনের জন্য UFW কনফিগার করুন:
```bash
sudo ufw allow 67
sudo ufw allow 53
sudo ufw default allow FORWARD
sudo ufw reload
```

যদি আপনি Firewalld ব্যবহার করেন:

```bash
sudo firewall-cmd --zone=trusted --add-port=67/udp
sudo firewall-cmd --zone=trusted --add-port=53/udp
sudo firewall-cmd --zone=trusted --add-forward
sudo firewall-cmd --zone=trusted --add-interface=waydroid0
sudo firewall-cmd --runtime-to-permanent
```
এই আর্টিকেল Gemini এবং [Archwiki](https://wiki.archlinux.org/title/Waydroid) এর সাহায্যে লেখা হয়েছে |
