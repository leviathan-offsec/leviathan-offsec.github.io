# Leviathan OffSec Site Setup Guide

Complete step-by-step guide to verify and fix the `leviathan.ac` domain with Namecheap + Cloudflare + GitHub Pages.

---

## Part 1: GitHub Pages Configuration

### Step 1.1: Enable GitHub Pages (Main Branch)

1. Go to: **https://github.com/leviathan-offsec/leviathan-offsec.github.io/settings/pages**
2. Under **"Build and deployment"** section:
   - **Source:** Select `Deploy from a branch`
   - **Branch:** Select `main`
   - **Folder:** Select `/ (root)`
3. Click **Save**
4. GitHub will show: `Your site is published at https://leviathan-offsec.github.io/`

**Expected:** Green checkmark + URL confirmation within 1-2 minutes.

---

## Part 2: DNS Setup (Namecheap → Cloudflare)

### Step 2.1: Point Namecheap Nameservers to Cloudflare

1. **Log in to Namecheap:** https://www.namecheap.com/
2. Go to **Domain List** → Click **leviathan.ac**
3. Under **Nameservers**, select:
   - **Nameserver Type:** `Custom DNS`
4. Replace existing nameservers with Cloudflare's:
   ```
   iris.ns.cloudflare.com
   nash.ns.cloudflare.com
   ```
5. Click **Save Changes**

**Wait:** 15-30 minutes for propagation (check status with `dig leviathan.ac NS`)

---

### Step 2.2: Configure Cloudflare DNS Records

1. **Log in to Cloudflare:** https://dash.cloudflare.com/
2. Select zone: **leviathan.ac**
3. Go to **DNS** → **Records** tab
4. **Delete any existing A records or CNAME records** (if any)

#### Add GitHub Pages A Records:

| Type | Name | IPv4 Address | TTL | Proxy Status |
|------|------|--------------|-----|--------------|
| A | leviathan.ac | 185.199.108.153 | Auto | DNS only |
| A | leviathan.ac | 185.199.109.153 | Auto | DNS only |
| A | leviathan.ac | 185.199.110.153 | Auto | DNS only |
| A | leviathan.ac | 185.199.111.153 | Auto | DNS only |

**Click Add Record for each IP**

#### Add www Subdomain (Optional but Recommended):

| Type | Name | Target | TTL | Proxy Status |
|------|------|--------|-----|--------------|
| CNAME | www | leviathan-offsec.github.io | Auto | DNS only |

**⚠️ CRITICAL:** Set **Proxy Status** to `DNS only` (gray cloud) — do NOT use Cloudflare's proxy for GitHub Pages.

---

## Part 3: GitHub Pages Custom Domain

### Step 3.1: Verify Repository CNAME File

1. Check that the `CNAME` file exists in repo root:
   - **File location:** `/CNAME`
   - **Content:**
     ```
     leviathan.ac
     ```

2. If missing, create it:
   ```bash
   echo "leviathan.ac" > CNAME
   git add CNAME
   git commit -m "Add CNAME for leviathan.ac"
   git push origin main
   ```

### Step 3.2: Set Custom Domain in GitHub

1. Go to: **https://github.com/leviathan-offsec/leviathan-offsec.github.io/settings/pages**
2. Under **Custom domain** section:
   - Enter: `leviathan.ac`
   - Click **Save**
3. GitHub will automatically:
   - Verify DNS ownership
   - Enable HTTPS (via Let's Encrypt)
   - Create GitHub Pages deployment workflow

**Expected:** 
- Green checkmark: "Your site is published at https://leviathan.ac"
- HTTPS enforcement available (toggle: "Enforce HTTPS")

---

## Part 4: DNS Verification Checklist

### Step 4.1: Verify A Records

Run these commands in terminal (Mac/Linux):

```bash
# Check all A records resolve to GitHub Pages IPs
dig leviathan.ac A

# Expected output:
# leviathan.ac. 3600 IN A 185.199.108.153
# leviathan.ac. 3600 IN A 185.199.109.153
# leviathan.ac. 3600 IN A 185.199.110.153
# leviathan.ac. 3600 IN A 185.199.111.153

# Check www CNAME (if added)
dig www.leviathan.ac CNAME

# Expected output:
# www.leviathan.ac. 3600 IN CNAME leviathan-offsec.github.io.
```

### Step 4.2: Verify Nameserver Delegation

```bash
# Check Cloudflare nameservers are active
dig leviathan.ac NS

# Expected output:
# leviathan.ac. 172800 IN NS iris.ns.cloudflare.com.
# leviathan.ac. 172800 IN NS nash.ns.cloudflare.com.
```

### Step 4.3: Cloudflare Verification

1. Go to Cloudflare Dashboard → **leviathan.ac** → **Overview**
2. Check **Nameserver Status:**
   - Should show: ✅ **"Nameservers are set up correctly"**
3. Check **SSL/TLS** → **Edge Certificates:**
   - Status: ✅ **Active Certificate**

---

## Part 5: HTTPS & Security Setup

### Step 5.1: Enable HTTPS in GitHub

1. Go to: **https://github.com/leviathan-offsec/leviathan-offsec.github.io/settings/pages**
2. Under **HTTPS** section:
   - Toggle: ✅ **"Enforce HTTPS"**

**Wait:** 1-2 minutes for certificate provisioning.

### Step 5.2: Cloudflare SSL/TLS Configuration

1. Go to Cloudflare Dashboard → **leviathan.ac** → **SSL/TLS**
2. Under **Overview**:
   - **SSL/TLS encryption mode:** Select `Flexible`
     - (GitHub Pages handles HTTPS termination)
3. Under **Edge Certificates**:
   - ✅ **Always use HTTPS** — toggle ON
   - ✅ **Automatic HTTPS Rewrites** — toggle ON

---

## Part 6: Validation & Testing

### Step 6.1: End-to-End Site Test

```bash
# Test HTTP redirect to HTTPS
curl -i http://leviathan.ac/
# Expected: 301/302 redirect to https://leviathan.ac/

# Test HTTPS access
curl -i https://leviathan.ac/
# Expected: 200 OK, HTML content

# Test certificate validity
openssl s_client -connect leviathan.ac:443 -servername leviathan.ac < /dev/null | grep -A 5 "subject="
# Expected: issuer = Let's Encrypt
```

### Step 6.2: Browser Test

1. Open: https://leviathan.ac/
2. Check address bar:
   - ✅ Secure lock icon (green padlock)
   - ✅ No certificate warnings
   - ✅ Address bar shows: `leviathan.ac`
3. Verify homepage loads:
   - ✅ "Snapshot your perimeter. Diff it weekly."
   - ✅ All navigation links functional
   - ✅ Research article links work

### Step 6.3: Research Article Test

1. Navigate to research article:
   - Click: "Read the research" or "All research"
   - Expected: `/research/` page loads
   - Or direct link: https://leviathan.ac/posts/80-days-reversing-iot-dvr.html
2. Verify:
   - ✅ Content renders correctly
   - ✅ Code blocks display properly
   - ✅ All images/assets load

---

## Part 7: DNS Propagation Timeline

| Time | Action | Status |
|------|--------|--------|
| T+0 | Nameservers changed at Namecheap | Pending |
| T+15-30m | Cloudflare receives authority for zone | In Progress |
| T+30-60m | Global DNS propagation begins | In Progress |
| T+1-2h | GitHub Pages detects custom domain | Active |
| T+2-5m (after GitHub config) | HTTPS certificate issued | Secure |
| T+2-4h | Full global propagation | ✅ Complete |

**Use:** https://www.whatsmydns.net/#A/leviathan.ac to check propagation across global DNS servers.

---

## Part 8: Troubleshooting

### Issue: "Domain verification failed" in GitHub

**Cause:** GitHub cannot resolve your domain to GitHub Pages IP addresses.

**Fix:**
1. Double-check Cloudflare A records (all 4 IPs must be present)
2. Ensure proxy status is `DNS only` (not proxied)
3. Wait 15 minutes and retry
4. If persists, check: `dig leviathan.ac A` and verify output

### Issue: HTTPS certificate not issued

**Cause:** Domain not properly verified within 24 hours.

**Fix:**
1. Verify Cloudflare CNAME at root is not interfering
2. Disable Cloudflare proxy (set to DNS only)
3. In GitHub settings, toggle "Enforce HTTPS" OFF, then back ON
4. Wait 5 minutes

### Issue: Site shows 404 or wrong content

**Cause:** GitHub Pages source not correctly configured or CNAME mismatch.

**Fix:**
1. Verify `/CNAME` file contains exactly: `leviathan.ac` (no www, no http://)
2. Go to Pages settings and confirm:
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/ (root)`
3. Force refresh: `Ctrl+Shift+R` (hard refresh, bypasses cache)

### Issue: www.leviathan.ac doesn't work

**Cause:** CNAME record not added for www subdomain.

**Fix:**
1. In Cloudflare DNS, add:
   - Type: `CNAME`
   - Name: `www`
   - Target: `leviathan-offsec.github.io`
2. Set proxy status to `DNS only`
3. Wait 5 minutes and test: https://www.leviathan.ac/

### Issue: Cloudflare shows "error 1016"

**Cause:** Cloudflare proxy enabled for GitHub Pages (misconfiguration).

**Fix:**
1. Go to Cloudflare → DNS Records
2. For all A records pointing to GitHub IPs, set **Proxy Status** → `DNS only` (gray cloud)
3. Save and wait 2 minutes

---

## Part 9: Ongoing Maintenance

### Monthly Checks

```bash
# Verify DNS still resolves correctly
dig +short leviathan.ac A

# Verify HTTPS certificate is valid (not expiring soon)
echo | openssl s_client -servername leviathan.ac -connect leviathan.ac:443 2>/dev/null | openssl x509 -noout -dates

# Check GitHub Pages deployment status
curl -s -o /dev/null -w "HTTP Status: %{http_code}\n" https://leviathan.ac/
```

### SSL Certificate Auto-Renewal

- **GitHub Pages:** Automatically renews Let's Encrypt certificates 30 days before expiry
- **No action needed** — GitHub handles renewal automatically
- Monitor: GitHub Pages settings page for any warnings

---

## Part 10: Quick Reference

### Critical Files & Settings

| Item | Location | Value |
|------|----------|-------|
| CNAME file | `/CNAME` | `leviathan.ac` |
| GitHub Pages source | Settings → Pages | `main / (root)` |
| GitHub custom domain | Settings → Pages | `leviathan.ac` |
| Cloudflare A records | DNS → Records | All 4 GitHub IPs |
| Cloudflare proxy | DNS → Records | DNS only (gray) |
| Namecheap nameservers | Domain settings | Cloudflare NS |
| HTTPS enforcement | Settings → Pages | ✅ Enabled |

### DNS Record Summary

```
leviathan.ac.    3600 IN A 185.199.108.153
leviathan.ac.    3600 IN A 185.199.109.153
leviathan.ac.    3600 IN A 185.199.110.153
leviathan.ac.    3600 IN A 185.199.111.153
www.leviathan.ac. 3600 IN CNAME leviathan-offsec.github.io.

; Nameservers (at Namecheap → Cloudflare):
leviathan.ac.    172800 IN NS iris.ns.cloudflare.com.
leviathan.ac.    172800 IN NS nash.ns.cloudflare.com.
```

---

## Part 11: Verification Completion Checklist

After completing all steps, verify:

- [ ] Namecheap nameservers changed to Cloudflare
- [ ] Cloudflare zone active (nameserver status OK)
- [ ] All 4 GitHub A records added in Cloudflare (proxy: DNS only)
- [ ] www CNAME added (optional, proxy: DNS only)
- [ ] CNAME file exists in repo root with correct domain
- [ ] GitHub Pages custom domain set to `leviathan.ac`
- [ ] GitHub Pages deployment source: `main / (root)`
- [ ] HTTPS enforcement enabled in GitHub
- [ ] DNS propagation verified with `dig` commands
- [ ] https://leviathan.ac/ loads with green lock
- [ ] https://leviathan.ac/posts/80-days-reversing-iot-dvr.html works
- [ ] Certificate valid (openssl check passes)
- [ ] Cloudflare SSL/TLS set to Flexible + HTTPS redirect enabled

---

## Support References

| Problem | Resource |
|---------|----------|
| GitHub Pages setup | https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages |
| Custom domain with GitHub Pages | https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-pages-site |
| Cloudflare DNS for GitHub Pages | https://developers.cloudflare.com/nameservers/zone-setup/full-setup/ |
| Namecheap custom nameservers | https://www.namecheap.com/support/knowledgebase/article.aspx/9593/46/how-to-change-dns-for-a-domain/ |
| HTTPS/Let's Encrypt with GitHub Pages | https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https |
| DNS propagation checker | https://www.whatsmydns.net/ |

---

**Last Updated:** 2026-10-01  
**Domain:** leviathan.ac  
**Repository:** leviathan-offsec/leviathan-offsec.github.io
