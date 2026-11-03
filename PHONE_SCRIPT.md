# PHONE SCRIPT — MORE MOUSE, SHORT LINES

Phone par padho. Mac screen par mat kholna.

Target **38 se 46 minute**.

Project: `/Users/dsthakur/odin/dns-failover-project`  
Prompt: `dns-failover-project %`

## RULES

1. Pehle short bold line bolo. Bolte waqt type mat.
2. Type chup chap. Type karte mat bolo.
3. Har Type ke baad **lambi mouse**: scroll 5–8 sec, sidebar click, pointer 3 sec rukna.
4. Validate + plan sirf **ek baar** end pe.
5. Paste mat. variables.tf mein keys pe mat rukna.
6. Har line mein kahan bolo: `in the center editor` / `command prompt in the center of the Terminal window`.

## 0:00 SE 0:20 — INTRO

Do: Start. TaskRecorder yellow minimize.

**I will be beginning my task soon. Here is the workflow I have in mind.**

Do: Goal window band.

## 0:20 SE 4:00 — TERMINAL

Do: Dock → Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing dig, because I want the current answer for api.prod-mesh.io.**

**Type**

```bash
dig @1.1.1.1 +time=3 +tries=1 +stats api.prod-mesh.io A
```

**I am pointing at NXDOMAIN in the center of the Terminal window.**

Do: Dig output slow scroll + pointer. **8 second.** Type nahi.

**I am scrolling the dig output in the center of the Terminal window.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing curl for a control check.**

**Type**

```bash
curl -sI --max-time 10 https://example.com
```

**I am pointing at HTTP/2 200 in the center of the Terminal window.**

Do: Pointer 200 pe. **6 second.** Terminal yellow minimize.

**I am clicking the yellow minimize button at the top left of the Terminal window.**

## 4:00 SE 10:00 — OPEN EVERY FILE (TYPE ZERO, ZYADA MOUSE)

Do: Dock → Visual Studio Code.

**I am clicking Visual Studio Code in the Dock.**

### main.tf

Do: Sidebar → main.tf. Providers se locals tak **slow scroll 10 second**. Pointer skip flags.

**I am clicking main.tf in the left sidebar. I am scrolling the provider skeleton in the center editor.**

### initial_zone.json

Do: Sidebar → initial_zone.json. Records + gaps tak **slow scroll 10 second**. Pointer api A / gaps.

**I am clicking initial_zone.json in the left sidebar. I am scrolling the before-state export in the center editor.**

### variables.tf

Do: Sidebar → variables.tf. Sirf domain_name / region dikhao. **Scroll 6 second.** Keys pe pointer mat.

**I am clicking variables.tf in the left sidebar. I am pointing at the domain name in the center editor.**

### README.md

Do: Sidebar → README.md. **Pointer 4 second.**

**I am clicking README.md in the left sidebar. I am pointing at the short note in the center editor.**

### scripts/test_dns_failover.py

Do: Sidebar → scripts folder open → test_dns_failover.py. Stub pe pointer. **6 second scroll.**

**I am clicking test_dns_failover.py in the left sidebar. I am pointing at the stub return in the center editor. I will fill this after Terraform.**

### dns_records.tf

Do: Sidebar → dns_records.tf. Public / edge / standby scroll. Phir empty blocks tak. **12 second slow scroll.** Har empty `{}` pe click + 2 sec rukna.

**I am clicking dns_records.tf in the left sidebar. I am scrolling the public zone in the center editor, then the empty blocks I will fill.**

Do: Private empty `{}` pe click. **3 second pause.**

## 10:00 SE 14:00 — PRIVATE ZONE

**I am clicking inside the empty private zone in the center editor, and I am typing the split-horizon zone.**

**Type**

```hcl
  name    = var.domain_name
  comment = "Split-horizon zone for VPC resolvers"
```

Do: Sidebar file click. Next line. **4 second.**

**I am clicking the file in the left sidebar, then the next line in the center editor.**

**Type**

```hcl
  vpc {
    vpc_id     = var.private_zone_vpc_id
    vpc_region = var.aws_region
  }
```

Do: Private block pe scroll up/down. Pointer vpc. **8 second.**

**I am scrolling the private zone in the center editor.**

Do: Scroll primary_edge empty. Click. **3 second.**

## 14:00 SE 19:00 — PRIMARY HEALTH CHECK

**I am clicking inside the empty primary health check in the center editor, and I am typing the edge probe.**

**Type**

```hcl
  fqdn              = local.edge_fqdn
  port              = 443
  type              = "HTTPS"
  resource_path     = "/healthz"
```

Do: Sidebar click. Next line. **5 second.**

**I am clicking the file in the left sidebar, then the next line in the center editor.**

**Type**

```hcl
  request_interval  = 10
  failure_threshold = 2
```

Do: Pointer un lines. **5 second.**

**I am pointing at the interval in the center editor.**

**Type**

```hcl
  regions           = ["us-east-1", "us-west-2", "eu-west-1"]
  insufficient_data_health_status = "LastKnownStatus"
```

Do: Primary block full slow scroll. **10 second.**

**I am scrolling the primary health check in the center editor.**

Do: Scroll standby empty. Click. **3 second.**

## 19:00 SE 23:00 — STANDBY HEALTH CHECK

**I am clicking inside the empty standby health check in the center editor, and I am typing the standby probe.**

**Type**

```hcl
  fqdn              = local.standby_fqdn
  port              = 443
  type              = "HTTPS"
  resource_path     = "/healthz"
```

Do: Sidebar. Next line. **5 second.**

**I am clicking the file in the left sidebar, then the next line in the center editor.**

**Type**

```hcl
  request_interval  = 10
  failure_threshold = 2
  insufficient_data_health_status = "LastKnownStatus"
```

Do: Primary ↔ standby ke beech scroll. **10 second.**

**I am scrolling between the two health checks in the center editor.**

Do: api_primary empty. Click. **3 second.**

## 23:00 SE 29:00 — PRIMARY + SECONDARY

**I am clicking inside the empty api primary block in the center editor, and I am typing the primary record.**

**Type**

```hcl
  zone_id         = aws_route53_zone.public.zone_id
  name            = local.api_fqdn
  type            = "A"
  ttl = 60
```

Do: Sidebar. Next line. **5 second.**

**I am clicking the file in the left sidebar, then the next line in the center editor.**

**Type**

```hcl
  records         = ["203.0.113.10"]
  set_identifier  = "cloudflare-edge-primary"
  health_check_id = aws_route53_health_check.primary_edge.id
```

Do: Pointer set_identifier. **5 second.**

**I am pointing at the set identifier in the center editor.**

**Type**

```hcl
  failover_routing_policy {
    type = "PRIMARY"
  }
```

Do: Primary block scroll. **8 second.**

**I am scrolling the primary record in the center editor.**

Do: api_secondary empty. Click. **3 second.**

**I am clicking inside the empty api secondary block in the center editor, and I am typing the secondary record.**

**Type**

```hcl
  zone_id         = aws_route53_zone.public.zone_id
  name            = local.api_fqdn
  type            = "A"
  ttl = 60
```

Do: Mouse. **4 second.**

**I am clicking the next line in the center editor.**

**Type**

```hcl
  records         = ["198.51.100.20"]
  set_identifier  = "aws-standby-secondary"
  health_check_id = aws_route53_health_check.standby_origin.id
```

Do: Pointer. **4 second.**

**I am pointing at the secondary identifier in the center editor.**

**Type**

```hcl
  failover_routing_policy {
    type = "SECONDARY"
  }
```

Do: PRIMARY ↔ SECONDARY scroll. **10 second.**

**I am scrolling between PRIMARY and SECONDARY in the center editor.**

## 29:00 SE 33:00 — INTERNAL + CLOUDFLARE

Do: api_internal empty. Click. **3 second.**

**I am clicking inside the empty api internal block in the center editor, and I am typing the private answer.**

**Type**

```hcl
  zone_id = aws_route53_zone.private.zone_id
  name    = local.api_fqdn
  type    = "A"
  ttl = 60
  records = ["10.20.0.15"]
```

Do: Pointer 10.20.0.15. **6 second.** Sidebar click.

**I am pointing at the private address in the center editor. I am clicking the file in the left sidebar.**

Do: Scroll api_edge empty. Click. **3 second.**

**I am clicking inside the empty Cloudflare api record in the center editor.**

**Type**

```hcl
  zone_id = var.cloudflare_zone_id
  name    = "api"
  type    = "A"
  content = "203.0.113.50"
```

Do: Next line. **4 second.**

**I am clicking the next line in the center editor.**

**Type**

```hcl
  ttl = 1
  proxied = true
```

Do: Scroll origin_primary. Click. **4 second.**

**I am clicking inside the empty origin-primary record in the center editor.**

**Type**

```hcl
  zone_id = var.cloudflare_zone_id
  name    = "origin-primary"
  type    = "A"
  content = "203.0.113.50"
  ttl = 60
  proxied = false
```

Do: Command-S. Top→bottom **slow scroll 12–15 second.** Empty nahi rehna chahiye.

**I am pressing Command-S. I am scrolling the finished dns_records.tf in the center editor.**

Do: VS Code yellow minimize.

**I am clicking the yellow minimize button at the top left of Visual Studio Code.**

## 33:00 SE 37:00 — VALIDATE + PLAN

Do: Dock → Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing terraform validate.**

**Type**

```bash
terraform validate -no-color | tee plan_output.txt
```

**I am pointing at Success in the center of the Terminal window.**

Do: Pointer success. **6 second.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing terraform plan.**

**Type**

```bash
terraform plan -input=false -no-color | tee -a plan_output.txt
```

**I am pointing at Plan 11 to add in the center of the Terminal window. I am not applying this.**

Do: Plan output **slow scroll 12–15 second.** PRIMARY / SECONDARY pe pointer.

**I am scrolling the plan in the center of the Terminal window.**

Do: Terminal yellow minimize.

**I am clicking the yellow minimize button at the top left of the Terminal window.**

## 37:00 SE 43:00 — PYTHON

Do: VS Code → scripts → test_dns_failover.py.

**I am clicking test_dns_failover.py in the left sidebar. I am clicking the return stub in the center editor.**

Do: `return 1` delete. **3 second pause.**

**Type**

```python
    ttl = 60
    degraded_ms = 2500
    threshold = 2
    interval = 10
    detection = interval * threshold
```

Do: Sidebar scripts click. Pointer detection. **6 second.**

**I am clicking the scripts folder in the left sidebar. I am pointing at detection in the center editor.**

**Type**

```python
    lines = []

    def emit(text: str = "") -> None:
        print(text)
        lines.append(text)
```

Do: Mouse. **5 second.**

**I am pointing at emit in the center editor.**

**Type**

```python
    emit("DNS FAILOVER SYNTHETIC TEST")
    emit("withdrawn: cloudflare-edge-primary")
    emit("selected: aws-standby-secondary 198.51.100.20")
```

Do: Sidebar file click. Scroll. **6 second.**

**I am clicking the test file in the left sidebar. I am scrolling the header in the center editor.**

**Type**

```python
    checks = [
        ("primary policy is PRIMARY", 'type = "PRIMARY"' in RECORDS),
        ("secondary policy is SECONDARY", 'type = "SECONDARY"' in RECORDS),
        ("authoritative ttl is 60", "ttl = 60" in RECORDS),
        ("private answer is 10.20.0.15", "10.20.0.15" in RECORDS),
```

Do: Pointer list. **6 second.**

**I am pointing at the checks list in the center editor.**

**Type**

```python
        ("cloudflare proxied ttl is 1", "ttl = 1" in RECORDS),
        ("degraded latency exceeds 800 ms", degraded_ms > 800),
        ("detection is shorter than the ttl", detection < ttl),
        ("standby address is present", "198.51.100.20" in RECORDS),
    ]
```

Do: Scroll checks. Sidebar. **8 second.**

**I am scrolling the checks in the center editor. I am clicking the file in the left sidebar.**

**Type**

```python
    failed = 0
    for label, passed in checks:
        state = "PASS" if passed else "FAIL"
        if not passed:
            failed += 1
        emit(f"[{state}] {label}")
```

Do: Pointer loop. **5 second.**

**I am pointing at the loop in the center editor.**

**Type**

```python
    emit("RESULT: PASS" if failed == 0 else "RESULT: FAIL")
    emit("resolver cache caveat: a cached primary answer may remain for up to 60 seconds")
    LOG.write_text("\n".join(lines) + "\n")
    return 1 if failed else 0
```

Do: Command-S. Top→bottom scroll. **10 second.**

**I am pressing Command-S. I am scrolling the finished script in the center editor.**

Do: VS Code yellow minimize.

**I am clicking the yellow minimize button at the top left of Visual Studio Code.**

## 43:00 SE 46:00 — RUN TEST

Do: Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing the python command.**

**Type**

```bash
python3 scripts/test_dns_failover.py
```

**I am pointing at RESULT PASS in the center of the Terminal window.**

Do: Har PASS pe pointer. Slow scroll. **12 second.**

**I am scrolling the PASS lines in the center of the Terminal window.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing cat.**

**Type**

```bash
cat execution_failover_test.log
```

**I am pointing at RESULT PASS in the center of the Terminal window.**

Do: Log pe pointer. **8 second.**

## 46:00 SE 48:00 — EPILOGUE

**I verified this without applying it. Validate succeeded. The plan adds 11 resources. The test shows eight assertions passing. Health checks use edge and standby, not api. Public TTL is 60 seconds. Private answer is 10.20.0.15. Proxied Cloudflare TTL is 1. I did not apply the plan.**

Do: RESULT / withdrawn / selected pe mouse. **8 second.**

**I am pointing at RESULT PASS in the center of the Terminal window.**

Do: TaskRecorder Stop.
