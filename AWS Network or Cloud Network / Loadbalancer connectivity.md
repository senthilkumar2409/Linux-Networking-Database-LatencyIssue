I have a real-time prod question, if a service A pod needs to talk to another service pod which is exposed by ALB, now we use alb dns to connect but why we need to define port as well? Is it mandatory?

**Short answer**: it's mandatory only if the ALB listener isn't on the default port for the scheme you're using.

**Why:** DNS resolves a hostname to an IP address only — it carries no port information. When you connect with just a hostname (e.g., `https://my-alb-1234.us-east-1.elb.amazonaws.com`), the client implicitly uses the default port for that scheme:
- `http://` → port 80
- `https://` → port 443

If your ALB listener is actually on 80 or 443, you don't need to specify the port — it's implied by the scheme.

**When you must specify it:** If the ALB listener is configured on a non-standard port (e.g., 8080, 8443, 3000), the client has no way to know that from DNS alone. You have to explicitly add `:8080` etc., or the connection will go to the default port and fail (connection refused, since nothing's listening there).

**In your prod scenario specifically:**
- Check what port(s) the ALB listener(s) are configured on (`aws elbv2 describe-listeners --load-balancer-arn ...`).
- If it's 80/443 with matching scheme, port is optional/cosmetic.
- If it's anything else, the port is required — and worth hardcoding into service A's config/env var rather than relying on defaults, since ALB listener ports can be a source of silent misconfig (e.g., someone changes the listener port during a Terraform update and now DNS still resolves fine but connections start failing).

One more gotcha worth checking: if service A is doing keep-alive/connection pooling to the ALB, make sure whatever port you configure is consistent across health checks and target group rules too — mismatches there cause intermittent 502s that look like a port issue but are actually target group registration issues.