# Life Itself newsletter

The home hero and shared premium Flowershow footer use native HTML forms. `_footer.html` replaces the default footer on every site page; no per-page footer markup or JavaScript is needed.

Forms post to `https://mailer.lifeitself.org/newsletter/v1` with `target=life-itself` and redirect to `https://lifeitself.org/newsletter/thanks`. The registered origin is `https://lifeitself.org`; the repository name is not a second deployment domain.

The exact consent promise beside both forms is: “Send me the Life Itself newsletter. Unsubscribe at any time.” The linked privacy policy is `/privacy-policy`. A successful request means it has been saved for processing, not that email has been delivered.

The shared CRM service records consent and syncs the subscription to Twenty and its dedicated Resend Topic. The configured sender is **Life Itself <hello@lifeitself.org>**. The domain was verified in Resend on 2026-10-27. No welcome email is sent by signup.

[Newsletter setup and operations](https://github.com/life-itself/crm/blob/main/guides/newsletter-signup.md) live in the CRM repository. Credentials stay server-side. Subscription tasks and verification evidence live in Beads `crm-au0` in that repository.

The newsletter archive remains available at https://news.lifeitself.org/. Research and other publications are separate. Creating this new signup destination does not import existing Substack subscribers or authorize newsletter broadcasts.
