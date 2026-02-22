# Care Before Crisis — Next Steps

## Done ✅
- [x] Landing page designed and built (`site/index.html`)
- [x] Deployed to Porkbun static hosting via FTP
- [x] Live at http://carebeforecrisis.org
- [x] Footer: "Isabella — Ann Arbor, MI"

## Site Polish (Quick Wins)
- [ ] Add an inline SVG favicon (no external file needed)
- [ ] Add og:image for social sharing previews
- [ ] Create a custom 404.html page
- [ ] Check Porkbun dashboard for SSL cert status — kick it if still pending

## Meeting Prep (Feb 22)
- [ ] Draft agenda/questions in `notes/meeting-2026-02-22.md`
  - What note format does her employer require?
  - Who reads her notes?
  - Does she need participant names kept or de-identified?
  - What does she hate about writing notes now?
  - Can she show a redacted sample note?
- [ ] Pull up carebeforecrisis.org on phone to demo

## App Deployment (After Meeting)
- [ ] Get an OpenAI API key (needed for Whisper + GPT-4o-mini)
- [ ] Deploy group-notes Flask app to Render (config files ready: `render.yaml`, `Procfile`)
- [ ] Test end-to-end: record → transcribe → structured notes
- [ ] Decide: beta.carebeforecrisis.org or separate Render URL?

## FTP Credentials (Porkbun)
- Host: pixie-ss1-ftp.porkbun.com
- User: carebeforecrisis.org
- Pass: ue*KdxEO8JhFcqpKe
- Upload command: `curl -T "site/index.html" "ftp://pixie-ss1-ftp.porkbun.com/" --user "carebeforecrisis.org:ue*KdxEO8JhFcqpKe"`
