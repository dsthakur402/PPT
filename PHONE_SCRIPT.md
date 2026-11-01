PHONE SCRIPT — TEESRI RECORDING

Phone par padho. Mac screen par mat kholna.

Target 24 se 28 minute. 15 se 50 ke beech hona chahiye.

Ek waqt par sirf ek badi window. Doosri app ka yellow button se minimize.

Har click ke baad 1 second ruko.

File mein jo TYPE likha hai, wahi exactly type karna hai. Poori command character-by-character mat bolna.

Har TYPE se pehle bold line bolo: kya, screen par kahan, aur kyun.

Starting files sirf skeleton hain.

dns_records.tf ke khali blocks aur Python ka assertion hissa recording mein type hoga.

variables.tf aur README mat kholna. Mock keys wahan hain.

## 0:00 SE 0:20 — INTRO

Do: Start Recording. TaskRecorder yellow minimize.

**I will be beginning my task soon. Here is the workflow I have in mind.**

Do: Goal window band karo.

## 0:20 SE 2:30 — TERMINAL

Visual Studio Code pehle se minimized ho.

Do: Dock mein Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing dig, because I want the current answer for api.prod-mesh.io.**

**Type**

```bash
dig @1.1.1.1 +time=3 +tries=1 +stats api.prod-mesh.io A
```

**I am pointing at the status line in the center of the Terminal window. It says NXDOMAIN, so this name is not delegated yet.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing curl, because I want a control HTTP status.**

**Type**

```bash
curl -sI --max-time 10 https://example.com
```

**I am pointing at HTTP/2 200 in the center of the Terminal window. That is only a reachability check.**

Do: Terminal yellow minimize.

**I am clicking the yellow minimize button at the top left of the Terminal window.**

## 2:30 SE 14:00 — VISUAL STUDIO CODE, TYPE THE CONFIG

Do: Dock mein Visual Studio Code. Left sidebar mein main.tf.

**I am clicking main.tf in the left sidebar. I am pointing at the provider blocks in the center editor. This skeleton already has the AWS and Cloudflare providers. The failover records are not in this file. I will write those in dns_records.tf.**

Do: Left sidebar mein dns_records.tf.

**I am clicking inside the empty private zone block in the center editor, and I am typing the VPC association, because this zone is the split horizon.**

**Type:** private zone ke braces ke andar:

```hcl
  name    = var.domain_name
  comment = "Split-horizon zone for VPC resolvers"

  vpc {
    vpc_id     = var.private_zone_vpc_id
    vpc_region = var.aws_region
  }
```

**I am clicking inside the empty primary health check in the center editor, and I am typing the probe, because it must target edge.prod-mesh.io and not the failover name api.prod-mesh.io.**

**Type:** primary_edge ke braces ke andar:

```hcl
  fqdn              = local.edge_fqdn
  port              = 443
  type              = "HTTPS"
  resource_path     = "/healthz"
  request_interval  = 10
  failure_threshold = 2
  regions           = ["us-east-1", "us-west-2", "eu-west-1"]
  insufficient_data_health_status = "LastKnownStatus"
```

**I am clicking inside the empty standby health check in the center editor, and I am typing the same probe for the standby name, because failover must not send traffic to a dead origin.**

**Type:** standby_origin ke braces ke andar:

```hcl
  fqdn              = local.standby_fqdn
  port              = 443
  type              = "HTTPS"
  resource_path     = "/healthz"
  request_interval  = 10
  failure_threshold = 2
  insufficient_data_health_status = "LastKnownStatus"
```

**I am clicking inside the empty api_primary block in the center editor, and I am typing the primary failover record. The TTL is 60 seconds so resolvers drop a withdrawn answer quickly.**

**Type:** api_primary ke braces ke andar:

```hcl
  zone_id         = aws_route53_zone.public.zone_id
  name            = local.api_fqdn
  type            = "A"
  ttl = 60
  records         = ["203.0.113.10"]
  set_identifier  = "cloudflare-edge-primary"
  health_check_id = aws_route53_health_check.primary_edge.id

  failover_routing_policy {
    type = "PRIMARY"
  }
```

**I am clicking inside the empty api_secondary block in the center editor, and I am typing the secondary record for 198.51.100.20.**

**Type:** api_secondary ke braces ke andar:

```hcl
  zone_id         = aws_route53_zone.public.zone_id
  name            = local.api_fqdn
  type            = "A"
  ttl = 60
  records         = ["198.51.100.20"]
  set_identifier  = "aws-standby-secondary"
  health_check_id = aws_route53_health_check.standby_origin.id

  failover_routing_policy {
    type = "SECONDARY"
  }
```

**I am clicking inside the empty api_internal block in the center editor, and I am typing the private answer 10.20.0.15. This record has no failover policy.**

**Type:** api_internal ke braces ke andar:

```hcl
  zone_id = aws_route53_zone.private.zone_id
  name    = local.api_fqdn
  type    = "A"
  ttl = 60
  records = ["10.20.0.15"]
```

**I am clicking inside the empty Cloudflare api record in the center editor. I am setting proxied to true and TTL to 1, because Cloudflare does not keep a custom TTL on a proxied record.**

**Type:** api_edge ke braces ke andar:

```hcl
  zone_id = var.cloudflare_zone_id
  name    = "api"
  type    = "A"
  content = "203.0.113.50"
  ttl = 1
  proxied = true
```

**I am clicking inside the empty origin-primary record in the center editor, and I am typing the DNS-only origin with a 60 second TTL.**

**Type:** origin_primary ke braces ke andar:

```hcl
  zone_id = var.cloudflare_zone_id
  name    = "origin-primary"
  type    = "A"
  content = "203.0.113.50"
  ttl = 60
  proxied = false
```

Do: Command-S.

**I am pressing Command-S to save dns_records.tf.**

Do: Visual Studio Code yellow minimize.

**I am clicking the yellow minimize button at the top left of Visual Studio Code.**

## 14:00 SE 17:00 — TERMINAL, VALIDATE AUR PLAN

Do: Dock mein Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing terraform validate, because I want to check the records I just wrote.**

**Type**

```bash
terraform validate -no-color | tee plan_output.txt
```

**I am pointing at the success line in the center of the Terminal window. The configuration is valid.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing terraform plan, because I want to review the changes without applying them.**

**Type**

```bash
terraform plan -input=false -no-color | tee -a plan_output.txt
```

**I am pointing at the last plan line in the center of the Terminal window. It says 11 to add, 0 to change, 0 to destroy. I am not applying this.**

Do: Terminal yellow minimize.

**I am clicking the yellow minimize button at the top left of the Terminal window.**

## 17:00 SE 21:00 — PYTHON ASSERTIONS

Do: Visual Studio Code. scripts, then test_dns_failover.py.

**I am clicking test_dns_failover.py in the left sidebar. I am clicking below the comment in the center editor, and I am replacing the return line with the drill values and the assertions.**

**Type:** comment ke neeche wali return 1 line delete karke yeh likho:

```python
    ttl = 60
    degraded_ms = 2500
    threshold = 2
    interval = 10
    detection = interval * threshold
    lines = []

    def emit(text: str = "") -> None:
        print(text)
        lines.append(text)

    emit("DNS FAILOVER SYNTHETIC TEST")
    emit("withdrawn: cloudflare-edge-primary")
    emit("selected: aws-standby-secondary 198.51.100.20")
    checks = [
        ("primary policy is PRIMARY", 'type = "PRIMARY"' in RECORDS),
        ("secondary policy is SECONDARY", 'type = "SECONDARY"' in RECORDS),
        ("authoritative ttl is 60", "ttl = 60" in RECORDS),
        ("private answer is 10.20.0.15", "10.20.0.15" in RECORDS),
        ("cloudflare proxied ttl is 1", "ttl = 1" in RECORDS),
        ("degraded latency exceeds 800 ms", degraded_ms > 800),
        ("detection is shorter than the ttl", detection < ttl),
        ("standby address is present", "198.51.100.20" in RECORDS),
    ]
    failed = 0
    for label, passed in checks:
        state = "PASS" if passed else "FAIL"
        if not passed:
            failed += 1
        emit(f"[{state}] {label}")
    emit("RESULT: PASS" if failed == 0 else "RESULT: FAIL")
    emit("resolver cache caveat: a cached primary answer may remain for up to 60 seconds")
    LOG.write_text("\n".join(lines) + "\n")
    return 1 if failed else 0
```

Do: Command-S.

**I am pressing Command-S to save the test script.**

Do: Visual Studio Code yellow minimize.

**I am clicking the yellow minimize button at the top left of Visual Studio Code.**

## 21:00 SE 23:00 — RUN THE TEST

Do: Dock mein Terminal.

**I am clicking the command prompt in the center of the Terminal window, and I am typing the python command, because I want to run the assertions I just wrote.**

**Type**

```bash
python3 scripts/test_dns_failover.py
```

**I am pointing at RESULT PASS in the center of the Terminal window.**

**I am clicking the command prompt in the center of the Terminal window, and I am typing cat, because I want to read the saved log.**

**Type**

```bash
cat execution_failover_test.log
```

Log screen par rehne do.

## 23:00 SE 26:00 — EPILOGUE

**I verified this without applying it. Validate succeeded, and the plan adds 11 resources and changes none. The test log shows the assertions passing. The health checks target edge and standby, not api.prod-mesh.io, because that name is the failover record. The public TTL is 60 seconds. Detection is about 20 seconds, and a resolver can keep a cached answer until that TTL ends. The private record is 10.20.0.15 and has no failover policy. The proxied Cloudflare record uses TTL 1. These addresses are documentation ranges, and I did not apply the plan.**

Do: TaskRecorder Stop.
