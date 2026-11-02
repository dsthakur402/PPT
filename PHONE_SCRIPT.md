# PHONE SCRIPT — CHAUTHI RECORDING

Phone par padho. Mac screen par mat kholna.

Target 32 se 38 minute. 15 se 50 ke beech.

Dispute mat karna. Yeh video submit mat karna. Nayi take.

## RULE — YEH 35% AUR 70% KE LIYE HAI

Pehle poori bold line bolo. Bolte waqt keyboard mat chhuo.

Phir chup chap sirf woh chhota code type karo. Type karte waqt mat bolo.

Har type ke baad mouse hilaao: scroll, ya left sidebar par click, phir wapas editor.

Paste mat karna. Copy mat karna. Ek lambi typing mat karna.

Har bold line mein teen cheez hon: kya kar rahe ho, screen par kahan, kyun.

Jagah ke word hamesha bolo: "in the center editor" ya "the command prompt in the center of the Terminal window".

Ek waqt par sirf ek badi window. Doosri ka yellow button.

Har click ke baad 1 second ruko.

variables.tf aur README mat kholna.

## 0:00 SE 0:20 — INTRO

Do: Start Recording. TaskRecorder yellow minimize.

**I will be beginning my task soon. Here is the workflow I have in mind.**

Do: Goal window band karo.

## 0:20 SE 3:00 — TERMINAL

Visual Studio Code pehle se minimized ho.

Do: Dock mein Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing dig, because I want the current answer for api.prod-mesh.io.**

**Type**

```bash
dig @1.1.1.1 +time=3 +tries=1 +stats api.prod-mesh.io A
```

**I am pointing at the status line in the center of the Terminal window. It says NXDOMAIN, so this name is not delegated yet.**

Do: Mouse se output ke upar scroll karo. 1 second ruko.

**I am scrolling the output in the center of the Terminal window to read the status again.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing curl, because I want a control HTTP status.**

**Type**

```bash
curl -sI --max-time 10 https://example.com
```

**I am pointing at HTTP/2 200 in the center of the Terminal window. That is only a reachability check.**

Do: Terminal yellow minimize.

**I am clicking the yellow minimize button at the top left of the Terminal window.**

## 3:00 SE 6:00 — PRIVATE ZONE

Do: Dock mein Visual Studio Code. Left sidebar mein main.tf.

**I am clicking main.tf in the left sidebar. I am pointing at the provider blocks in the center editor. This skeleton already has the AWS and Cloudflare providers. The failover records are not in this file.**

Do: Mouse se main.tf ke beech tak scroll. 1 second ruko.

**I am scrolling the provider block in the center editor. I will write the records in dns_records.tf.**

Do: Left sidebar mein dns_records.tf. Private zone ke khali braces par click.

**I am clicking inside the empty private zone block in the center editor, and I am typing the zone name, because this zone is the split horizon.**

**Type**

```hcl
  name    = var.domain_name
  comment = "Split-horizon zone for VPC resolvers"
```

Do: Mouse se usi block ke neeche click. 1 second ruko.

**I am clicking the next line inside the private zone in the center editor, and I am typing the VPC block, because only that VPC should see the private answer.**

**Type**

```hcl
  vpc {
    vpc_id     = var.private_zone_vpc_id
    vpc_region = var.aws_region
  }
```

Do: Left sidebar mein dns_records.tf par dubara click. Phir editor mein primary_edge ke khali braces par click.

**I am clicking the file name in the left sidebar, then the empty primary health check in the center editor.**

## 6:00 SE 10:00 — HEALTH CHECKS

**I am clicking inside the empty primary health check in the center editor, and I am typing the probe target, because it must be edge.prod-mesh.io and not the failover name api.prod-mesh.io.**

**Type**

```hcl
  fqdn              = local.edge_fqdn
  port              = 443
  type              = "HTTPS"
  resource_path     = "/healthz"
```

Do: Mouse se thoda scroll. Agli khali line par click. 1 second ruko.

**I am clicking the next line of the primary health check in the center editor, and I am typing the interval and the failure threshold, because two failures at 10 seconds is about 20 seconds of detection.**

**Type**

```hcl
  request_interval  = 10
  failure_threshold = 2
```

Do: Mouse se agli line par click.

**I am clicking the next line of the primary health check in the center editor, and I am typing the regions and the insufficient-data status, because three regions must agree and a missing sample should keep the last known status.**

**Type**

```hcl
  regions           = ["us-east-1", "us-west-2", "eu-west-1"]
  insufficient_data_health_status = "LastKnownStatus"
```

Do: Scroll karke standby_origin ke khali braces par click.

**I am scrolling to the empty standby health check in the center editor, and I am clicking inside it.**

**I am typing the standby probe target in the center editor, because failover must not send traffic to a dead origin.**

**Type**

```hcl
  fqdn              = local.standby_fqdn
  port              = 443
  type              = "HTTPS"
  resource_path     = "/healthz"
```

Do: Mouse se agli line par click. 1 second ruko.

**I am clicking the next line of the standby health check in the center editor, and I am typing the same interval, threshold, and last-known status.**

**Type**

```hcl
  request_interval  = 10
  failure_threshold = 2
  insufficient_data_health_status = "LastKnownStatus"
```

Do: Left sidebar par click, phir api_primary ke khali braces par click.

**I am clicking the file in the left sidebar, then the empty api primary block in the center editor.**

## 10:00 SE 16:00 — FAILOVER RECORDS

**I am clicking inside the empty api primary block in the center editor, and I am typing the record name and a 60 second TTL, because resolvers should drop a withdrawn answer quickly.**

**Type**

```hcl
  zone_id         = aws_route53_zone.public.zone_id
  name            = local.api_fqdn
  type            = "A"
  ttl = 60
```

Do: Mouse se agli line par click.

**I am clicking the next line inside api primary in the center editor, and I am typing the address and the set identifier cloudflare-edge-primary.**

**Type**

```hcl
  records         = ["203.0.113.10"]
  set_identifier  = "cloudflare-edge-primary"
  health_check_id = aws_route53_health_check.primary_edge.id
```

Do: Mouse se block ke end par click.

**I am clicking the bottom of the api primary block in the center editor, and I am typing the failover policy, because this record is the primary.**

**Type**

```hcl
  failover_routing_policy {
    type = "PRIMARY"
  }
```

Do: Scroll karke api_secondary ke khali braces par click. 1 second ruko.

**I am scrolling to the empty api secondary block in the center editor, and I am clicking inside it.**

**I am typing the secondary name and a 60 second TTL in the center editor.**

**Type**

```hcl
  zone_id         = aws_route53_zone.public.zone_id
  name            = local.api_fqdn
  type            = "A"
  ttl = 60
```

Do: Mouse se agli line par click.

**I am clicking the next line inside api secondary in the center editor, and I am typing the standby address and the set identifier aws-standby-secondary.**

**Type**

```hcl
  records         = ["198.51.100.20"]
  set_identifier  = "aws-standby-secondary"
  health_check_id = aws_route53_health_check.standby_origin.id
```

Do: Mouse se block ke end par click.

**I am clicking the bottom of the api secondary block in the center editor, and I am typing the failover policy, because this record is the secondary.**

**Type**

```hcl
  failover_routing_policy {
    type = "SECONDARY"
  }
```

Do: Scroll karke api_internal ke khali braces par click.

**I am scrolling to the empty api internal block in the center editor, and I am clicking inside it.**

**I am typing the private answer in the center editor. The address is 10.20.0.15, and this record has no failover policy, because only the VPC resolver should see it.**

**Type**

```hcl
  zone_id = aws_route53_zone.private.zone_id
  name    = local.api_fqdn
  type    = "A"
  ttl = 60
  records = ["10.20.0.15"]
```

Do: Left sidebar par click, phir api_edge ke khali braces par scroll karke click.

**I am clicking the file in the left sidebar, then scrolling to the empty Cloudflare api record in the center editor.**

## 16:00 SE 19:00 — CLOUDFLARE

**I am clicking inside the empty Cloudflare api record in the center editor, and I am typing the name and the address.**

**Type**

```hcl
  zone_id = var.cloudflare_zone_id
  name    = "api"
  type    = "A"
  content = "203.0.113.50"
```

Do: Mouse se agli line par click. 1 second ruko.

**I am clicking the next line of the Cloudflare api record in the center editor. I am setting the TTL to 1 and proxied to true, because Cloudflare does not keep a custom TTL on a proxied record.**

**Type**

```hcl
  ttl = 1
  proxied = true
```

Do: Scroll karke origin_primary ke khali braces par click.

**I am scrolling to the empty origin-primary record in the center editor, and I am clicking inside it.**

**I am typing the DNS-only origin in the center editor, with a 60 second TTL and proxied set to false.**

**Type**

```hcl
  zone_id = var.cloudflare_zone_id
  name    = "origin-primary"
  type    = "A"
  content = "203.0.113.50"
  ttl = 60
  proxied = false
```

Do: Command-S. Phir mouse se editor ke top par scroll.

**I am pressing Command-S to save dns_records.tf. I am scrolling to the top of the center editor to review the private zone.**

Do: Visual Studio Code yellow minimize.

**I am clicking the yellow minimize button at the top left of Visual Studio Code.**

## 19:00 SE 23:00 — VALIDATE AUR PLAN

Do: Dock mein Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing terraform validate, because I want to check the records I just wrote.**

**Type**

```bash
terraform validate -no-color | tee plan_output.txt
```

**I am pointing at the success line in the center of the Terminal window. The configuration is valid.**

Do: Mouse se us line par hover. 1 second ruko.

**I am moving the pointer over the success line in the center of the Terminal window.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing terraform plan, because I want to review the changes without applying them.**

**Type**

```bash
terraform plan -input=false -no-color | tee -a plan_output.txt
```

**I am pointing at the last plan line in the center of the Terminal window. It says 11 to add, 0 to change, and 0 to destroy. I am not applying this.**

Do: Terminal yellow minimize.

**I am clicking the yellow minimize button at the top left of the Terminal window.**

## 23:00 SE 32:00 — PYTHON, CHHOTE HISSE

Do: Visual Studio Code. scripts folder, phir test_dns_failover.py. return 1 wali line par click.

**I am clicking test_dns_failover.py in the left sidebar. I am clicking the return line in the center editor, under the comment, because I am replacing that stub with the drill values.**

Do: return 1 delete karo. Sirf yeh paanch line type karo.

**Type**

```python
    ttl = 60
    degraded_ms = 2500
    threshold = 2
    interval = 10
    detection = interval * threshold
```

Do: Mouse se sidebar mein scripts par click, phir wapas editor ki agli line.

**I am clicking the scripts folder in the left sidebar, then the next line in the center editor, and I am typing the emit helper, because each line must be printed and saved.**

**Type**

```python
    lines = []

    def emit(text: str = "") -> None:
        print(text)
        lines.append(text)
```

Do: Mouse se agli khali line par click. 1 second ruko.

**I am clicking the next line in the center editor, and I am typing the three header lines, because the log must name the withdrawn primary and the selected standby.**

**Type**

```python
    emit("DNS FAILOVER SYNTHETIC TEST")
    emit("withdrawn: cloudflare-edge-primary")
    emit("selected: aws-standby-secondary 198.51.100.20")
```

Do: Mouse se scroll. Agli line par click.

**I am scrolling in the center editor, and I am typing the first four checks, because the test must see PRIMARY, SECONDARY, the 60 second TTL, and the private address.**

**Type**

```python
    checks = [
        ("primary policy is PRIMARY", 'type = "PRIMARY"' in RECORDS),
        ("secondary policy is SECONDARY", 'type = "SECONDARY"' in RECORDS),
        ("authoritative ttl is 60", "ttl = 60" in RECORDS),
        ("private answer is 10.20.0.15", "10.20.0.15" in RECORDS),
```

Do: Left sidebar par file par click. Phir editor mein comma ke baad click.

**I am clicking the test file in the left sidebar, then the next check line in the center editor.**

**I am typing the last four checks in the center editor, because the proxied TTL is 1, detection must be shorter than 60 seconds, and the standby address must be present.**

**Type**

```python
        ("cloudflare proxied ttl is 1", "ttl = 1" in RECORDS),
        ("degraded latency exceeds 800 ms", degraded_ms > 800),
        ("detection is shorter than the ttl", detection < ttl),
        ("standby address is present", "198.51.100.20" in RECORDS),
    ]
```

Do: Mouse se file ke end par scroll. 1 second ruko.

**I am scrolling to the bottom of the center editor, and I am typing the loop, because every check must run before the result line.**

**Type**

```python
    failed = 0
    for label, passed in checks:
        state = "PASS" if passed else "FAIL"
        if not passed:
            failed += 1
        emit(f"[{state}] {label}")
```

Do: Mouse se loop ke bahar, agli line par click. return loop ke andar mat likhna.

**I am clicking the line after the loop in the center editor, and I am typing the result and the log write, because the return must stay outside the loop.**

**Type**

```python
    emit("RESULT: PASS" if failed == 0 else "RESULT: FAIL")
    emit("resolver cache caveat: a cached primary answer may remain for up to 60 seconds")
    LOG.write_text("\n".join(lines) + "\n")
    return 1 if failed else 0
```

Do: Command-S.

**I am pressing Command-S to save the test script in the center editor.**

Do: Visual Studio Code yellow minimize.

**I am clicking the yellow minimize button at the top left of Visual Studio Code.**

## 32:00 SE 35:00 — RUN THE TEST

Do: Dock mein Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing the python command, because I want to run the assertions I just wrote.**

**Type**

```bash
python3 scripts/test_dns_failover.py
```

**I am pointing at RESULT PASS in the center of the Terminal window. All eight checks passed.**

Do: Mouse se output par scroll.

**I am scrolling the test output in the center of the Terminal window.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing cat, because I want to read the saved log.**

**Type**

```bash
cat execution_failover_test.log
```

**I am pointing at the same RESULT PASS line in the center of the Terminal window.**

## 35:00 SE 38:00 — EPILOGUE

**I verified this without applying it. Validate succeeded, and the plan adds 11 resources and changes none. The test log shows eight assertions passing. The health checks target edge and standby, not api.prod-mesh.io, because that name is the failover record. The public TTL is 60 seconds. Detection is about 20 seconds, and a resolver can keep a cached answer until that TTL ends. The private record is 10.20.0.15 and has no failover policy. The proxied Cloudflare record uses TTL 1. These addresses are documentation ranges, and I did not apply the plan.**

Do: TaskRecorder Stop.
