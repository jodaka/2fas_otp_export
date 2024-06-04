# 2FAS app backup qr codes generator

## Running locally

You'll need to have `git` and `node` installed. Strictly speaking you could get away without git by (downloading zip)[https://github.com/jodaka/2fas_otp_export/archive/refs/heads/main.zip], but node is still required

1) Clone repo
```
git clone https://github.com/jodaka/2fas_otp_export.git
```
2) Install dependencies
```
cd 2fas_otp_export
npm install
```
3) Run site locally
```
npm run dev -- --open
```
After that command browser should open local web page that is able to generate qr codes from 2FAS backup.


## DEMO SITE

If you aren't comfortable with command line and node but still need to get qr codes, you can use demo site. While reading your backup and generation of qr codes happen locally on your computer (nothing is being sent or logged anywhere), it's not wise to trust me or any other random person on the internet when it happens to security.

1) Open 2FAS app and make a backup without password (Settings » 2FAS Backup » Export to file » Export)
2) Open [https://kudris.com/2fas_export](https://kudris.com/2fas_export) and choose your backup
3) Page will generate qr codes for every token in your backup file
