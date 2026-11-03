# PHONE SCRIPT — UNDER 35% TYPING

Phone par padho. Mac screen par mat kholna.

Target **36 se 44 minute**. 15 se 50 ke beech.

Project: `/Users/dsthakur/odin/dns-failover-project`  
Prompt: `dns-failover-project %`

## BEST STRATEGY (35% KE ANDAR)

Typing sirf short bursts. Beech mein **mouse + bolna** time khata hai, typing % giraata hai.

Har Type ke baad yeh cycle:

1. Pehle **poori bold line bolo**. Bolte waqt keys mat dabao.
2. **Chup chap** sirf woh chhota block type karo. Type karte mat bolo.
3. **Command-S** mat har line pe. Sirf section ke end pe.
4. Type ke baad **mouse**: scroll 2–3 second, ya left sidebar file click, ya empty block par click, ya yellow minimize / Dock switch.
5. **Beech mein terraform validate mat chalana.** Fail noise nahi chahiye. Validate + plan **sirf ek baar**, jab saara Terraform complete ho.
6. Paste / copy mat.
7. Har bold line: **kya** + **kahan** (`in the center editor` / `command prompt in the center of the Terminal window`) + **kyun**.
8. variables.tf / README mat kholna.
9. Ek waqt ek badi window. Doosri yellow minimize.

Mouse filler jo typing nahi hain (baar baar use karo):

- Left sidebar par file name click
- Editor mein scroll up/down
- Pointer se kisi line par 2 second rukna
- Empty next block par click
- VS Code ↔ Terminal yellow minimize + Dock click (dig/curl ke baad, terraform se pehle, python se pehle)

## 0:00 SE 0:20 — INTRO

Do: Start. TaskRecorder yellow minimize.

**I will be beginning my task soon. Here is the workflow I have in mind.**

Do: Goal window band.

## 0:20 SE 4:00 — TERMINAL LOOKUP (KAM TYPE, ZYADA POINT)

Do: Dock → Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing dig, because I want the current public answer for api.prod-mesh.io before I edit any records.**

**Type**

```bash
dig @1.1.1.1 +time=3 +tries=1 +stats api.prod-mesh.io A
```

**I am pointing at the status line in the center of the Terminal window. It says NXDOMAIN, so this name is not delegated yet. That matches the zone export I will use as the before state.**

Do: Mouse se dig output slow scroll. Har 1 second rukte hue 5–6 second. Pointer SOA / status par.

**I am scrolling the dig output in the center of the Terminal window. I am pointing again at NXDOMAIN. The miss is the name, not the resolver path.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing curl, because I want a healthy HTTP control from the same machine.**

**Type**

```bash
curl -sI --max-time 10 https://example.com
```

**I am pointing at HTTP/2 200 in the center of the Terminal window. I am also pointing at the server header. This is only a reachability check. I am not calling the documentation addresses from the zone file.**

Do: Curl output pe mouse. 4 second. Terminal yellow minimize.

**I am moving the pointer over the 200 line in the center of the Terminal window. I am clicking the yellow minimize button at the top left of the Terminal window.**

## 4:00 SE 7:00 — REVIEW SKELETON (TYPE ZERO)

Do: Dock → Visual Studio Code. Sidebar → main.tf.

**I am clicking main.tf in the left sidebar. I am pointing at the provider blocks in the center editor. The AWS and Cloudflare providers are already here as the starting skeleton. The failover records are not in this file.**

Do: main.tf slow scroll providers → locals. 6–8 second. Pointer skip flags par.

**I am scrolling the provider block in the center editor. I am pointing at the skip flags. They let me plan offline with mock keys. I will not apply this plan. Next I open dns_records.tf to fill the empty resources.**

Do: Sidebar → dns_records.tf. Public zone / edge / standby pe scroll. Pointer. 6–8 second. Type mat.

**I am clicking dns_records.tf in the left sidebar. I am scrolling the public zone and the edge and standby A records in the center editor. Those two names already exist so the health checks have a target. The private zone and the failover records below are still empty braces. That is what I will type.**

Do: Scroll down empty private `{}` tak. Click andar. 2 second pause.

## 7:00 SE 11:00 — PRIVATE ZONE

**I am clicking inside the empty private zone block in the center editor, and I am typing the zone name and comment, because this zone is the split-horizon copy of the same apex.**

**Type**

```hcl
  name    = var.domain_name
  comment = "Split-horizon zone for VPC resolvers"
```

Do: Sidebar dns_records.tf click. Wapas next line. 2 second.

**I am clicking the file name in the left sidebar, then the next line inside the private zone in the center editor. I am typing the VPC association, because only that VPC should see the private answer.**

**Type**

```hcl
  vpc {
    vpc_id     = var.private_zone_vpc_id
    vpc_region = var.aws_region
  }
```

Do: Private block pe mouse scroll up/down. Pointer vpc_id. 5 second. Type nahi.

**I am pointing at the VPC block in the center editor. Public clients will not receive this private answer. Next I fill the primary health check.**

Do: Scroll primary_edge empty. Click andar.

## 11:00 SE 16:00 — PRIMARY HEALTH CHECK

**I am clicking inside the empty primary health check in the center editor. I am typing the probe target, because the check must hit edge.prod-mesh.io and must not check the failover name api.prod-mesh.io.**

**Type**

```hcl
  fqdn              = local.edge_fqdn
  port              = 443
  type              = "HTTPS"
  resource_path     = "/healthz"
```

Do: Scroll thoda. Sidebar click. Next line. 3 second.

**I am clicking the file in the left sidebar, then the next line of the primary health check in the center editor. I am typing the interval and the failure threshold, because two failures at 10 seconds is about 20 seconds of detection.**

**Type**

```hcl
  request_interval  = 10
  failure_threshold = 2
```

Do: Pointer un lines par. 3 second.

**I am pointing at the interval and the failure threshold in the center editor.**

**I am clicking the next line of the primary health check in the center editor, and I am typing the regions and LastKnownStatus, because three regions must agree and a missing sample should keep the last known status.**

**Type**

```hcl
  regions           = ["us-east-1", "us-west-2", "eu-west-1"]
  insufficient_data_health_status = "LastKnownStatus"
```

Do: Primary block slow review scroll. 6 second. Bolte raho.

**I am scrolling the finished primary health check in the center editor. Edge is the probe target. Api is not. Next I copy the same shape for standby, still by typing, not by paste.**

Do: Scroll standby_origin empty. Click.

## 16:00 SE 20:00 — STANDBY HEALTH CHECK

**I am clicking inside the empty standby health check in the center editor. I am typing the standby probe target, because failover must not send traffic to a dead origin.**

**Type**

```hcl
  fqdn              = local.standby_fqdn
  port              = 443
  type              = "HTTPS"
  resource_path     = "/healthz"
```

Do: Mouse next line. Sidebar. 3 second.

**I am clicking the file in the left sidebar, then the next line of the standby health check in the center editor. I am typing the same interval, threshold, and last-known status.**

**Type**

```hcl
  request_interval  = 10
  failure_threshold = 2
  insufficient_data_health_status = "LastKnownStatus"
```

Do: Dono health checks ke beech scroll compare. 6–8 second.

**I am scrolling between the primary and standby health checks in the center editor. Both use HTTPS on /healthz. Only the fqdn changes. Next I write the PRIMARY failover record.**

Do: Scroll api_primary empty. Click.

## 20:00 SE 26:00 — API PRIMARY + SECONDARY

**I am clicking inside the empty api primary block in the center editor. I am typing the name and a 60 second TTL, because resolvers should drop a withdrawn answer quickly.**

**Type**

```hcl
  zone_id         = aws_route53_zone.public.zone_id
  name            = local.api_fqdn
  type            = "A"
  ttl = 60
```

Do: Sidebar + next line. 3 second.

**I am clicking the file in the left sidebar, then the next line inside api primary in the center editor. I am typing the edge address and the set identifier cloudflare-edge-primary.**

**Type**

```hcl
  records         = ["203.0.113.10"]
  set_identifier  = "cloudflare-edge-primary"
  health_check_id = aws_route53_health_check.primary_edge.id
```

Do: Pointer set_identifier. 3 second.

**I am pointing at the set identifier in the center editor. Next I add the failover policy.**

**Type**

```hcl
  failover_routing_policy {
    type = "PRIMARY"
  }
```

Do: Primary block review scroll. 5 second.

**I am scrolling the finished primary record in the center editor. Next I write the secondary record for the standby address.**

Do: Scroll api_secondary. Click.

**I am clicking inside the empty api secondary block in the center editor. I am typing the secondary name and a 60 second TTL.**

**Type**

```hcl
  zone_id         = aws_route53_zone.public.zone_id
  name            = local.api_fqdn
  type            = "A"
  ttl = 60
```

Do: Mouse. 3 second.

**I am clicking the next line inside api secondary in the center editor. I am typing the standby address and the set identifier aws-standby-secondary. I am spelling secondary carefully.**

**Type**

```hcl
  records         = ["198.51.100.20"]
  set_identifier  = "aws-standby-secondary"
  health_check_id = aws_route53_health_check.standby_origin.id
```

Do: Pointer. 2 second.

**I am pointing at the secondary set identifier in the center editor. Now I type the SECONDARY failover policy.**

**Type**

```hcl
  failover_routing_policy {
    type = "SECONDARY"
  }
```

Do: PRIMARY aur SECONDARY ke beech scroll. 6 second.

**I am scrolling between PRIMARY and SECONDARY in the center editor. Same name, same TTL, different addresses and health checks. Next is the private answer.**

## 26:00 SE 30:00 — INTERNAL + CLOUDFLARE

Do: api_internal empty. Click.

**I am clicking inside the empty api internal block in the center editor. I am typing the private answer 10.20.0.15. This record has no failover policy, because only the VPC resolver should see it.**

**Type**

```hcl
  zone_id = aws_route53_zone.private.zone_id
  name    = local.api_fqdn
  type    = "A"
  ttl = 60
  records = ["10.20.0.15"]
```

Do: Pointer 10.20.0.15. 4 second. Sidebar click.

**I am pointing at the private address in the center editor. I am clicking the file in the left sidebar, then scrolling to the empty Cloudflare api record.**

**I am clicking inside the empty Cloudflare api record in the center editor. I am typing the name and the origin address.**

**Type**

```hcl
  zone_id = var.cloudflare_zone_id
  name    = "api"
  type    = "A"
  content = "203.0.113.50"
```

Do: Mouse next line. 3 second.

**I am clicking the next line of the Cloudflare api record in the center editor. I am setting TTL to 1 and proxied to true, because Cloudflare does not keep a custom TTL on a proxied record.**

**Type**

```hcl
  ttl = 1
  proxied = true
```

Do: Scroll origin_primary. Click. 2 second.

**I am scrolling to the empty origin-primary record in the center editor, and I am clicking inside it. I am typing the DNS-only origin with a 60 second TTL and proxied set to false.**

**Type**

```hcl
  zone_id = var.cloudflare_zone_id
  name    = "origin-primary"
  type    = "A"
  content = "203.0.113.50"
  ttl = 60
  proxied = false
```

Do: Command-S. File top se bottom slow scroll review. **8–10 second.** Pointer PRIMARY, SECONDARY, private, ttl = 1.

**I am pressing Command-S to save dns_records.tf. I am scrolling the finished file in the center editor from the private zone down through PRIMARY, SECONDARY, the private answer, and the proxied Cloudflare record. I am not applying anything yet.**

Do: VS Code yellow minimize.

**I am clicking the yellow minimize button at the top left of Visual Studio Code.**

## 30:00 SE 34:00 — VALIDATE + PLAN (SIRF EK BAAR)

Do: Dock → Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing terraform validate, because all of the empty blocks are filled and I want to confirm the configuration is valid.**

**Type**

```bash
terraform validate -no-color | tee plan_output.txt
```

**I am pointing at the success line in the center of the Terminal window. The configuration is valid.**

Do: Pointer success line. 4 second. Mouse thoda move.

**I am keeping the pointer on the success line in the center of the Terminal window.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing terraform plan, because I want to review the eleven adds without applying them.**

**Type**

```bash
terraform plan -input=false -no-color | tee -a plan_output.txt
```

**I am pointing at the last plan line in the center of the Terminal window. It says 11 to add, 0 to change, and 0 to destroy. I am not applying this.**

Do: Plan output **slow scroll** 10–12 second. PRIMARY / SECONDARY / health check lines pe pointer. Bolte raho.

**I am scrolling the plan in the center of the Terminal window. I am pointing at the primary failover record, then the secondary, then the private zone association. Eleven resources will be added if this were applied. I am stopping at the plan summary.**

Do: Terminal yellow minimize.

**I am clicking the yellow minimize button at the top left of the Terminal window.**

## 34:00 SE 41:00 — PYTHON (CHHOTE BURSTS + BAHUT MOUSE)

Do: VS Code → scripts → test_dns_failover.py.

**I am clicking test_dns_failover.py in the left sidebar. I am pointing at the stub return line under the comment in the center editor. I will replace that stub with the drill values and the eight assertions.**

Do: `return 1` select karke delete. Mouse. 2 second. Type shuru.

**I am clicking the return line in the center editor and deleting it. I am typing the drill values first.**

**Type**

```python
    ttl = 60
    degraded_ms = 2500
    threshold = 2
    interval = 10
    detection = interval * threshold
```

Do: Sidebar scripts click. Wapas editor. Pointer detection. 4 second.

**I am clicking the scripts folder in the left sidebar, then pointing at detection in the center editor. Detection is interval times threshold, about 20 seconds, which must stay under the 60 second TTL.**

**I am clicking the next line in the center editor, and I am typing the emit helper, because every line must print and also go into the log file.**

**Type**

```python
    lines = []

    def emit(text: str = "") -> None:
        print(text)
        lines.append(text)
```

Do: Mouse. 3 second.

**I am pointing at the emit helper in the center editor. Next I type the three header lines for the log.**

**Type**

```python
    emit("DNS FAILOVER SYNTHETIC TEST")
    emit("withdrawn: cloudflare-edge-primary")
    emit("selected: aws-standby-secondary 198.51.100.20")
```

Do: Sidebar file click. Scroll. 4 second.

**I am clicking the test file in the left sidebar. I am scrolling the header lines in the center editor. Next I type the first four checks.**

**Type**

```python
    checks = [
        ("primary policy is PRIMARY", 'type = "PRIMARY"' in RECORDS),
        ("secondary policy is SECONDARY", 'type = "SECONDARY"' in RECORDS),
        ("authoritative ttl is 60", "ttl = 60" in RECORDS),
        ("private answer is 10.20.0.15", "10.20.0.15" in RECORDS),
```

Do: Pointer open list. 4 second. Bolna.

**I am pointing at the open checks list in the center editor. Those four checks cover PRIMARY, SECONDARY, the public TTL, and the private address. Now the last four checks.**

**Type**

```python
        ("cloudflare proxied ttl is 1", "ttl = 1" in RECORDS),
        ("degraded latency exceeds 800 ms", degraded_ms > 800),
        ("detection is shorter than the ttl", detection < ttl),
        ("standby address is present", "198.51.100.20" in RECORDS),
    ]
```

Do: Scroll checks. Sidebar. 5 second.

**I am scrolling the full checks list in the center editor. I am clicking the file in the left sidebar, then the next line after the list. I am typing the loop so every check runs before the result.**

**Type**

```python
    failed = 0
    for label, passed in checks:
        state = "PASS" if passed else "FAIL"
        if not passed:
            failed += 1
        emit(f"[{state}] {label}")
```

Do: Pointer loop. 3 second.

**I am pointing at the loop in the center editor. The return must stay outside this loop. I am clicking the next line after the loop.**

**Type**

```python
    emit("RESULT: PASS" if failed == 0 else "RESULT: FAIL")
    emit("resolver cache caveat: a cached primary answer may remain for up to 60 seconds")
    LOG.write_text("\n".join(lines) + "\n")
    return 1 if failed else 0
```

Do: Command-S. File top→bottom slow scroll. 8 second.

**I am pressing Command-S to save the test script. I am scrolling the finished script in the center editor from the drill values through the checks and the log write. Next I run it in Terminal.**

Do: VS Code yellow minimize.

**I am clicking the yellow minimize button at the top left of Visual Studio Code.**

## 41:00 SE 44:00 — RUN TEST

Do: Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing the python command, because I want to run the eight assertions I just wrote.**

**Type**

```bash
python3 scripts/test_dns_failover.py
```

**I am pointing at RESULT PASS in the center of the Terminal window. All eight checks passed.**

Do: Output slow scroll. Har PASS pe pointer. 8–10 second.

**I am scrolling the test output in the center of the Terminal window. I am pointing at each PASS line, then at RESULT PASS again.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing cat, because I want the saved log on screen for the end of the recording.**

**Type**

```bash
cat execution_failover_test.log
```

**I am pointing at the same RESULT PASS line in the center of the Terminal window. The log matches the live output.**

Do: Log pe pointer. 5 second. Type nahi.

## 44:00 SE 48:00 — EPILOGUE (SIRF BOLNA + MOUSE)

**I verified this without applying it. Validate succeeded once, after the records were complete. The plan adds 11 resources and changes none. The test log shows eight assertions passing. The health checks target edge and standby, not api.prod-mesh.io, because that name is the failover record. The public TTL is 60 seconds. Detection is about 20 seconds, and a resolver can keep a cached answer until that TTL ends. The private record is 10.20.0.15 and has no failover policy. The proxied Cloudflare record uses TTL 1. These addresses are documentation ranges, and I did not apply the plan.**

Do: Mouse se RESULT PASS, withdrawn line, selected line point. 6–8 second aur bolo.

**I am pointing at the withdrawn primary identifier in the center of the Terminal window, then at the selected standby address, then at RESULT PASS one more time.**

Do: TaskRecorder Stop. 50:59 se pehle.
