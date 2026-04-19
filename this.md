# App Store Survival Guide

This document is a practical guide for indie developers and small teams. It compiles facts from Apple's official reports, court filings, reporting by TechCrunch, MacRumors, 9to5Mac, and the experience of real developers. Nothing is made up â€” every fact is backed by a source.

---

## 1. Statistics: how often Apple removes apps and accounts

### Apple App Store

| Metric | 2022 | 2023 | 2024 |
|---|---|---|---|
| Submissions | â€” | â€” | 7,770,000 |
| Rejected | â€” | â€” | 1,930,000 (~25%) |
| Apps removed | 186,195 | 116,117 | 82,509 |
| Accounts removed | 428,487 | 117,843 | 146,747 |
| Of those, for fraud | â€” | â€” | 146,583 (99.9%) |
| Legitimate caught in the net | â€” | â€” | ~164 (0.011%) |
| Appeals filed | â€” | â€” | 8,132 |
| Successful appeals | â€” | â€” | 225 (2.8%) |

In 2024, one in four submissions failed review. That doesn't mean every fourth app is bad: the same app is often submitted multiple times after revisions.

The trend of app removals is going down (from 186K in 2022 to 82K in 2024) because Apple cleaned out most of the junk in earlier years.

Of the 146,747 accounts removed in 2024, 99.9% were fraudsters. Legitimate developers caught up in the sweep numbered around 164 out of roughly 1,500,000 active accounts. Odds: 0.011%.

Main reasons for app removals:

| Reason for removal | 2023 | 2024 |
|---|---|---|
| Design guidelines[^1] | 76,887 | 42,252 |
| Fraud | 35,245 | 38,315 |

Apple prevented more than $2 billion in fraudulent transactions in 2024. At the end of 2024 there were 1,961,596 apps on the App Store; by January 2026, about 1,912,383.

Review is handled by 577 full-time specialists and more than 150 expert editors. Every app goes through a two-stage review: first an automated scanner, then a human.

Appeals[^2] filed in 2024: 8,132; only 225 were successful â€” 2.8%. Near zero.

[^1]: **Guidelines** â€” Apple's official rules and recommendations that every app must meet to be allowed into the App Store. Full text: developer.apple.com/app-store/review/guidelines/
[^2]: **Appeal** â€” a formal complaint to the Apple App Review Board, filed by a developer who believes their app was rejected or removed unfairly.

### Google Play

Google Play also fights junk, but at a different scale:

| Metric | 2023 | 2024 | 2025 |
|---|---|---|---|
| Apps blocked/rejected | 2,280,000 | 2,360,000 | 1,750,000+ |
| Accounts blocked | â€” | 158,000 | 80,000+ |

Since 2024, AI has been involved in 92% of Google Play app reviews. Each app is subjected to more than 10,000 automated security checks. From the start of 2024 through April 2025, the number of apps on Google Play dropped from roughly 3.4 million to about 1.8 million â€” a 47% mass cleanup.

### What this means for an indie developer

The odds that Apple removes a legitimate developer's account: 0.011% per year. Near zero. But if it does happen, the odds of a successful appeal are only 2.8%. So the strategy is simple: don't end up in a situation where you need an appeal.

---

## 2. Vibe coding and Apple's automated review (spring 2026)

### What happened

Vibe coding[^3] is when an app is written not by a human but by an AI (Claude Code, Cursor, Replit, and similar tools). The person describes what they want; the AI generates the code. Since late 2025 this has become a mass phenomenon, and App Store submissions spiked 84% in a single quarter. Q1 2026 saw 235,800 submissions â€” an all-time record.

Apple physically cannot review everything manually with its 577 specialists, so it has strengthened the first, automated stage of review.

[^3]: **Vibe coding** â€” a term describing the practice of building apps with AI tools, where the developer describes the desired result in natural language and the code is generated automatically.

### How app review currently works

Review happens in two stages.

Stage one â€” automated scanner. It checks code signing[^4], looks for calls to private APIs[^5] Apple forbids, checks for the Privacy Manifest[^6] file, and analyzes which third-party SDKs[^7] are embedded and what they do. If the scanner finds no problems, the app moves to stage two.

Stage two â€” a human. The reviewer[^8] installs the app on a real device (not a simulator), walks through the whole user flow from first launch to the core feature, checks payment screens, onboarding[^9], and whether the app matches its store listing and all of Apple's guidelines.

[^4]: **Code signing** â€” a cryptographic signature that confirms the app was created by a specific developer and was not modified after the build. Without a proper signature Apple will not accept the app.
[^5]: **Private APIs** â€” internal iOS functions Apple uses in its own apps but forbids third parties from using. Calling them is a guaranteed rejection.
[^6]: **Privacy Manifest** â€” the PrivacyInfo.xcprivacy file, mandatory since 2024. The developer lists what data the app collects, which APIs it uses, and why. Details in the review section.
[^7]: **SDK** (Software Development Kit) â€” a set of tools from a third-party developer that gets embedded in an app. For example, Firebase SDK for analytics, Stripe SDK for payments, Amplitude SDK for behavior tracking.
[^8]: **Reviewer** â€” an Apple employee who manually checks an app before it is allowed into the App Store.
[^9]: **Onboarding** â€” the screens a user sees when first launching an app. Usually they explain what the app does and how to use it.

### Problem: the automation makes mistakes

As of spring 2026, the automated scanner has a lot of false positives. Concrete examples:

| What the scanner sees | What it thinks | What it actually is |
|---|---|---|
| Analytics SDK (Adjust, AppsFlyer) | The app has advertising | The SDK is only used to track installs |
| Firebase Anonymous Auth[^10] | There's authentication â€” needs a demo video | The user doesn't enter anything; auth happens in the background |
| Call to an AI API | Suspicious activity | The app is just calling an API to generate content |

The fix is simple: explain everything up front in the App Review Notes[^11] field. A detailed template is in the review section below.

[^10]: **Firebase Anonymous Auth** â€” a Firebase (Google) feature that creates an anonymous user identifier without a login or password. Used to save progress before the user registers.
[^11]: **App Review Notes** â€” a text field in App Store Connect (the developer dashboard) that is read by both the automation and the human reviewer. Put explanations about the app here.

### The review queue has grown a lot

| Period | iOS app review time |
|---|---|
| Before spring 2026 | 24â€“48 hours |
| Marchâ€“April 2026 | 7â€“30 days |

The Apple Developer Forums have dozens of threads from developers whose apps sit for weeks in "Waiting for Review." Requests for expedited review[^12] often go unanswered.

The cause is the same submission spike from vibe coding. Apple is hiring more reviewers, but can't keep up.

[^12]: **Expedited review** â€” a special request to Apple for priority review. Approved only in two cases: a critical bug in a published app, or tied to an event with a specific date.

### Guideline 2.5.2 â€” the main reason AI apps get pulled

Guideline 2.5.2 says an app must be self-contained[^13]. It is forbidden to download, interpret, or execute code that changes the app's functionality after it has been reviewed.

Vibe-coding apps violate this rule directly: they generate and run new apps inside themselves, effectively creating an "App Store inside the App Store."

Who has already been removed or blocked:

| App | What happened | Date |
|---|---|---|
| Anything | Removed ($100M valuation, 700K+ users) | March 30, 2026 |
| Replit | Updates blocked | March 2026 |
| Vibecode | Updates blocked | March 2026 |
| Botify AI (Ex-Human) | Removed, $500K frozen | March 2026 |
| Photify AI (Ex-Human) | Removed, $500K frozen | March 2026 |

Apps that generate text content via an API (not executable code) are not affected by this rule.

[^13]: **Self-contained** â€” Apple's principle that an app must contain all of its functionality at the time of review and must not change behavior after approval.

### The Ex-Human case: lawsuit against Apple (April 3, 2026)

Ex-Human built two apps: Botify AI (AI character chat) and Photify AI (avatar generation). Apple removed both apps citing "dishonest or fraudulent activity" with no specific explanation and froze $500,000 of the company's revenue in its account.

Ex-Human filed suit, accusing Apple of arbitrary rule enforcement and conflict of interest: the removal of Photify AI coincided with Apple's marketing push for Image Playground â€” Apple's own AI image generation product.

At the time of writing, the case is ongoing.

### New technical requirements as of April 2026

Starting April 28, 2026, every app submitted to the App Store must be built with Xcode 26 using the iOS 26 SDK. This doesn't mean the app will only run on the newest iPhones â€” but it must be compiled with the new toolchain.

Every third-party SDK embedded in an app must ship a Privacy Manifest (PrivacyInfo.xcprivacy) describing what data the SDK collects and why. No file â€” rejection. The full list of SDKs for which this is mandatory is published at developer.apple.com/support/third-party-SDK-requirements/

If an app uses AI, you must explain clearly: which model processes data, what exactly is sent to the AI provider's servers, and the user must be able to see when content was created by AI.

Apple also no longer accepts vague wording in the App Privacy section. You need to describe in detail: what specific data is collected, why, and whether it's linked to the user's identity. Apps that hide or misrepresent AI capabilities are rejected more often.

### More on review delays

Most of the time is spent in the queue, not in the review itself. The typical pattern: the app sits for weeks in "Waiting for Review," and once it finally reaches a reviewer ("In Review"), the check takes 1â€“2 days.

Critically important: resubmitting resets your queue position. If your app is already waiting and you re-upload, you go to the back of the line. This makes delays even worse.

Expedited review[^12] is limited â€” a developer has about 2 expedited-review requests per year. As submission volume grows, more developers use them, so even an approved request can take longer than usual.

### Guideline 4.7 â€” mini apps and HTML5 (November 2025 update)

In November 2025, Apple closed a loophole in Guideline 4.7. Previously, some developers embedded HTML5 mini apps and mini games inside their apps to bypass standard review. Now:

| Requirement | Summary |
|---|---|
| Full guideline compliance | Each mini app is reviewed like a regular app |
| No native APIs | Mini apps can only use WebKit and JavaScriptCore |
| Age restrictions | Age verification is required for content above the host app's rating |
| Content manifest | A mandatory index of all mini apps with universal links |

For most apps this isn't critical â€” but useful to know if you plan to embed interactive elements via a WebView.

### Alternative stores: EU and Japan

Besides the App Store, there are already legal alternative iOS distribution channels:

| Region | Law | What's allowed | Since iOS version |
|---|---|---|---|
| European Union | DMA (Digital Markets Act) | Alternative stores, web install, third-party payments | iOS 17.4+ (March 2024) |
| Japan | MSCA (Mobile Software Competition Act) | Alternative stores, third-party payments (no web install) | iOS 26.2 (December 2025) |

Already operating in the EU: AltStore PAL, Setapp Mobile, Epic Games Store. Apple notarizes[^38] every app in alternative stores â€” an automated check for malware and basic compliance. This is lighter than a full App Store review.

In Japan, Apple opened alternative marketplaces in December 2025, but without web installation (unlike the EU).

If you register in the EU, you get access to alternative distribution channels as a fallback for App Store problems.

Starting in early 2026, Apple moved the EU to a single business model: instead of the Core Technology Fee (a flat fee per install), it now charges CTC (a commission on digital goods and services) that applies across all channels â€” App Store, alternative stores, and web install.

[^38]: **Notarization** â€” Apple's automated check of every iOS app distributed through alternative stores in the EU and Japan. Scans for malicious code, viruses, and basic security compliance. Less strict than a full App Store review.

### WWDC 2026 and what comes next

WWDC 2026 takes place June 8â€“12, 2026. Apple will announce iOS 27, macOS 27, and other updates. The conference is focused on "advances in AI" â€” new requirements for AI apps are possible.

Separately: Apple is moving analytics from the Sales and Trends section to a new Analytics section in App Store Connect. Starting in mid-2026, 100+ new metrics will appear. The old Sales and Trends section will start winding down, with full migration by the end of 2027.

Do not submit to review the week before WWDC (June 1â€“7, 2026) â€” the queue will be at its peak.

### New design and performance requirements

Apple has tightened UI quality requirements:

| Requirement | Details |
|---|---|
| Load time | If the app takes longer than 10 seconds from splash screen â€” rejection |
| Dynamic Type[^39] | The app must support user-adjustable text size |
| Dark Mode | Dark-theme support recommended (not yet mandatory, but reviewers check) |
| Spelling and grammar | Errors in UI text can trigger rejection |
| Contrast | Text must be readable, with sufficient contrast against the background |

[^39]: **Dynamic Type** â€” an iOS feature that lets users change text size across all apps via device settings. Apple requires apps to scale text correctly when this setting changes.

---

## 3. How to pass review

### App Review Notes â€” what to write in the reviewer field

App Review Notes is a text field in App Store Connect seen by both the automated scanner and the human reviewer. Write in English, short and to the point. The goal is to preempt any questions the automation or the human might have.

A template for an app that uses AI (fill in your own data):

```
ABOUT THIS APP:
[Name] is a [category] app that [what it does].
It does NOT generate or execute any code.

AI USAGE:
- AI models ([which APIs]) generate [content type] only
- User data sent to AI: [what exactly is sent]
- No personal data is shared with AI services
- Users see a consent screen before first AI interaction

ADVERTISING:
- This app contains NO advertising
- Analytics SDK is used for product improvement only
- No user tracking, no IDFA usage

AUTHENTICATION:
- Sign in with Apple + Email login available
  Test account: [email] / [password]

IN-APP PURCHASES:
- Subscription: $X.XX/month or $XX.XX/year
- Restore Purchases button is on the subscription screen
- Free tier available with limited [feature]
```

Core principle: anything that might look suspicious to the automated scanner should be explained in advance. Using AI â€” write what data you send and where. Embedded analytics â€” state there is no advertising and no user tracking. Authentication present â€” provide a working test account.

### Test account for the reviewer

If the app has any form of authentication, you must create a working test account with full access to all features, including paid ones. Put login and password in App Review Notes.

Important: the account must be working at the time of review. If the auth token expires, the server goes down, or the password is changed while the reviewer is testing â€” rejection. Given current review delays (up to 30 days), make sure the account does not expire.

### Demo video

Apple may request a video in several cases: if the automation can't figure out how the app works, if onboarding is complex, or if the app uses AI. Better to prepare one in advance and attach it to the submission rather than wait for a request.

Requirements: screen recording from a real device (not a simulator), 30â€“60 seconds long, showing the full core user path from launch to result. Max file size: 500 MB.

### Privacy Manifest â€” the privacy file

Privacy Manifest is the PrivacyInfo.xcprivacy file added to the Xcode project. In it you must specify:

Which system APIs from the Required Reason API[^14] list the app uses. For example, UserDefaults (storing settings on the device) or NSFileManager (file operations). For each API you must provide a reason from Apple's approved list.

What data the app collects: analytics, user requests, progress, and so on.

Which external servers the app contacts: api.openai.com, api.anthropic.com, your own backend.

Which third-party SDKs are embedded and what they're used for.

The full list of SDKs that must ship with a Privacy Manifest is published at developer.apple.com/support/third-party-SDK-requirements/

[^14]: **Required Reason API** â€” a list of iOS system functions for which Apple requires an explicit reason in the Privacy Manifest. Introduced to fight device fingerprinting, where apps covertly collect unique device traits to track users.

### AI disclosure

Since late 2025, Apple treats AI usage as a separate category of data processing. What you need to do:

Show a consent screen before the first AI request. The screen must name the specific provider processing the data â€” not an abstract "service provider," but a specific name: "Anthropic Claude" or "OpenAI GPT." The user must tap a consent button before the app sends anything at all to AI servers.

Example: on first use of an AI feature, a screen appears with text like "Your request will be processed by Anthropic Claude to generate content. Only the text of your request is sent. No personal data is transmitted. Continue?" â€” and a consent button.

In your Privacy Policy[^15] on the website and inside the app, describe: what data is sent to the AI service, how it is processed, and how it is deleted.

In App Privacy Labels[^16] in App Store Connect, flag that data is transmitted to third parties.

Showing consent once â€” on first use â€” is enough. You don't need to ask on every request.

[^15]: **Privacy Policy** â€” a legal document describing what data the app collects, how it is processed, who it is shared with, and how the user can delete it. Apple requires Privacy Policy to be available both on the website and inside the app.
[^16]: **App Privacy Labels** â€” a card on the App Store showing users what data the app collects. Filled out by the developer in App Store Connect and verified during review.

### Sign in with Apple â€” when it's required

Sign in with Apple[^17] is required if the app offers sign-in via any third-party service: Google, Facebook, VK, Telegram, Twitter. If the app uses only its own authentication (email + password), Sign in with Apple is not required.

Important UI requirement: the Sign in with Apple button must be the same size and at the same level as buttons for other sign-in methods. You cannot make the Google button large and the Apple button small at the bottom of the screen.

[^17]: **Sign in with Apple** â€” Apple's authentication system letting users sign in with their Apple ID. Apple requires this option if the app offers any other third-party sign-in method (Google, Facebook, etc.). The rule has been in effect since 2020.

### What to do if the app is rejected

Read the reason. Apple always cites the specific guideline that was violated. Reply in the Resolution Center[^18] politely and on the facts â€” explain the situation or describe what was fixed. Don't silently resubmit without explanation. Reply within 24 hours if possible â€” by developers' observations, this really does speed up the re-review.

If Resolution Center doesn't help, file an appeal with the App Review Board at developer.apple.com/app-store/review/appeal/ â€” the case is reviewed by senior reviewers, usually within 5â€“7 business days.

If you get two rejections in a row for the same reason, it's better to redesign the feature than to keep arguing. Apple will not change its position.

[^18]: **Resolution Center** â€” the section in App Store Connect where a developer can correspond with the reviewer, ask questions about a rejection, and provide additional information.

### Expedited Review

Request via developer.apple.com/contact/app-store/?topic=expedite. Apple approves expedited review only in two situations:

First â€” a critical bug in an already published app that breaks core functionality for users. Attach steps to reproduce the bug.

Second â€” the app is tied to a specific event with a fixed date (conference, holiday, product launch).

If approved, review takes 4â€“12 hours. But don't abuse it â€” if you send requests often without a real reason, Apple will start ignoring them.

### TestFlight before the first submission

TestFlight[^19] is Apple's beta testing tool. Developers report that apps that went through TestFlight beta with five or more testers before first submission tend to get approved faster. Apple can see the TestFlight testing history and understands the app wasn't thrown together overnight.

[^19]: **TestFlight** â€” Apple's official platform for distributing beta versions of apps to testers before publishing on the App Store. Allows up to 10,000 external testers.

### Common first-submission mistakes

The most common reasons apps get rejected on first submission[^20]:

| Mistake | Why it gets rejected |
|---|---|
| Placeholder text[^21] (Lorem Ipsum, "TODO") | App looks unfinished |
| Broken Privacy Policy link | Page doesn't open or returns an error |
| Screenshots don't match the UI | Metadata doesn't match reality (Guideline 2.3) |
| No Restore Purchases[^22] button | Apple requires it for all subscription apps |
| Price in description â‰  price in app | Metadata mismatch |
| Crash on iPad | Even if the app is iPhone-only, the reviewer may test on iPad |
| Privacy Policy only on website | Must also be available inside the app |
| No account deletion feature | Mandatory since 2022 for all apps with registration |
| AI feature without consent screen | Violation of Guideline 5.1.2(i) |
| Backend down during review | Reviewer opens the app, server doesn't respond â€” rejection |

[^20]: **Submit** â€” sending the app for review in the App Store via App Store Connect.
[^21]: **Placeholder text** â€” temporary stand-in text a developer inserts during development and must replace with real content before publishing.
[^22]: **Restore Purchases** â€” a button that lets a user restore a previously paid subscription after reinstalling the app or on a new device. Apple requires this button in all apps with paid features.

### When to submit

Not on Friday â€” review may start Monday, and if the reviewer finds a problem you'll be fixing it on the weekend. Not right before WWDC[^23] (typically early June) â€” the queue is at its peak. Best time: Tuesday or Wednesday.

With the current delays of spring 2026, plan for 2â€“4 weeks from submission to publication. The app's backend must stay up the whole time, because the reviewer can start checking at any moment.

[^23]: **WWDC** (Worldwide Developers Conference) â€” Apple's annual developer conference, usually the first week of June. Apple announces new iOS, macOS, and other platform versions then.

---

## 4. Real account termination cases

Below are ten documented account-termination stories. Each one is real and cited.

| # | Who | Date | Cause | Outcome |
|---|---|---|---|---|
| 1 | Sarafan Mobile | Sept 2023 | Linked to someone else's account | $108K frozen, lawsuit lost, founded a new company |
| 2 | Reazy | Feb 2026 | AI used to fill out forms | Removed before publication, appeal impossible |
| 3 | Appstun | Aug 2024 | Watch faces "mislead" users | Entire account removed |
| 4 | Buttfield-Addison | Dec 2025 | Compromised gift card | Entire Apple ID blocked, restored via press |
| 5 | Epic Games | March 2024 | Alternative EU store | Restored in 2 days via EU regulators |
| 6 | Delivery (Africa) | June 2025 | Employee action | Entire business lost access |
| 7 | App buyer | Dec 2025 | Previous owner's violations | Account removed |
| 8 | Trademark dispute | Nov 2025 | Many resubmissions | Account removed |
| 9 | Former employee (Google) | 2024 | Personal account linked to work | Restored months later |
| 10 | Anything AI | March 2026 | Code generation (2.5.2) | $100M company, removed, web still works |

Details of each case below.

### Case 1: Viktor Seraleev and Sarafan Mobile (September 2023)

Viktor had six apps for content creators with monthly revenue of $33,680 (MRR[^24]). Apple blocked his account, linking him to a previously blocked account of a company called Softeam-2. It was a different company with different people â€” just a similar name and some overlaps Apple detected automatically.

Apple froze $108,878 in his account. Viktor corresponded with Apple for eight months, started a Change.org petition, and his story hit #1 on Hacker News. He eventually filed suit in federal court in California â€” and lost. The judge concluded Apple was acting within the developer agreement. The money was returned; the account was not. Viktor built a new company and over time grew to $60,000 MRR.

Lesson: Apple links accounts by company name, IP addresses, devices, and people. One match with a banned account â€” ban.

[^24]: **MRR** (Monthly Recurring Revenue) â€” a standard metric for subscription businesses.

### Case 2: Reazy / Ben Whitfield (February 2026)

Ben Whitfield, a former Pentagon economist, spent three years building a text-to-speech app for students with dyslexia (a reading disability). He used the Claude Cowork AI tool to fill out forms in App Store Connect.

Account terminated before publication â€” zero users, zero revenue. Filing an appeal is physically impossible, because the appeal form requires an active developer account. On Google Play the app works without issue.

Lesson: don't use AI tools to fill out App Store Connect forms. Apple can detect machine-generated text and treat it as fraud.

### Case 3: Appstun / WWDC Student Winner (August 2024)

A developer who won Apple's student competition at WWDC 2021 built an app with custom watch faces for Apple Watch. Apple decided the animations misled users â€” they look like real watch faces but technically are not.

Apple removed not just the app but the entire developer account.

Lesson: if Apple believes users could be deceived by an app's appearance, it removes the account without warning or negotiation.

### Case 4: Dr. Paris Buttfield-Addison (December 2025)

Scientist, game studio founder, author of more than 20 O'Reilly books on Apple development. He activated a $500 Apple gift card â€” the card turned out to be compromised (someone had opened the packaging in-store and recorded the code).

Apple blocked his entire Apple ID â€” 25 years of data, terabytes of photos, iMessage, all devices turned into bricks. The account was restored only after the story hit Daring Fireball, AppleInsider, and 9to5Mac â€” the biggest outlets in the Apple world.

Lesson: don't activate gift cards bought from unfamiliar places. And above all â€” public pressure and the press remain the only leverage on Apple in cases like this.

### Case 5: Epic Games in Sweden (March 2024)

Apple terminated the Epic Games account in Sweden three weeks after approving it â€” for trying to launch an alternative app store in the EU. The account was restored in two days after pressure from EU regulators.

Lesson: registering in the European Union provides real legal protection through the DMA[^25].

[^25]: **DMA** (Digital Markets Act) â€” the EU's Digital Markets Act, in force since 2024. It requires large platforms (Apple, Google, Meta) to open their ecosystems: allow alternative app stores, third-party payments, and give developers the right to an explanation for app removals.

### Case 6: African delivery company (June 2025)

A food delivery platform that had been operating for more than two years. One employee on a shared office computer took actions Apple considered violations. The entire account was terminated. Couriers, restaurants, and shops â€” every partner â€” lost access to the platform.

Lesson: restrict access to the developer account. One employee can destroy an entire business.

### Case 7: app buyer (December 2025)

A developer bought an app through Apple's official App Transfer[^26] process. Apple terminated his account for the previous owner's violations, who had repeatedly broken the guidelines.

Lesson: when you buy someone else's app, you buy its violation history. Before purchasing, you need to check the seller account's reputation.

[^26]: **App Transfer** â€” Apple's official procedure for moving an app from one developer account to another. Used when selling apps.

### Case 8: trademark dispute (November 2025)

A developer repeatedly resubmitted the app with edits because of a trademark[^27] dispute. Apple treated the multiple resubmissions as an attempt to circumvent the review process and terminated the account.

Lesson: don't resubmit the same app many times in a row. Apple reads it as suspicious behavior.

[^27]: **Trademark** â€” a registered brand name or logo. Using someone else's trademark in the app name or description violates Apple's guidelines.

### Case 9: former employee (Google Play, 2024)

An employee left three years earlier. Their personal Google account violated Google Play policies. Google automatically linked the personal account to the company account (because the employee once had access) and terminated the company account. It was restored later, but the process took several months.

Lesson: when an employee leaves, immediately revoke all their access to the developer account and any related services.

### Case 10: Anything AI (March 2026)

An app for generating other apps with AI. Company valuation: $100 million, more than 700,000 users. Apple removed it for violating Guideline 2.5.2 â€” generating executable code inside an app.

The Anything team tried a compromise: open results not inside the app but in the browser. Apple removed it anyway. The web version continues to work.

Lesson: you cannot build an "App Store inside the App Store." Apple protects its distribution monopoly.

---

## 5. The guidelines people trip over most often

| Guideline | Gist | What to watch for |
|---|---|---|
| 2.1 | App must work | Test on real devices + iPad |
| 2.3 | Metadata = reality | Screenshots only of the real UI |
| 2.5.2 | No executing downloaded code | Doesn't apply to content apps (text â‰  code) |
| 3.1.1 | Purchases only via StoreKit | Follow payment rules, include Restore Purchases |
| 3.1.2 | Subscriptions without dark patterns | Honest paywall, price shown large |
| 4.2 | Minimum functionality | Native features required, not just WebView |
| 4.3 | No spam or clones | One app, unique design |
| 5.1.1 | Transparent data collection | Fill out Privacy Labels, list all SDKs |
| 5.1.2(i) | Disclose AI provider | Consent screen with provider name |
| 5.1.2 | ATT for IDFA | Only needed if you have ads or IDFA |

Details on each:

### Guideline 2.1 â€” the app must work

The app crashes, doesn't load, has broken screens. Apple tests on its own devices â€” if it works for you but not for the reviewer, rejection. Test on real devices, not just the Xcode simulator. Don't forget iPad â€” even if the app is iPhone-only, the reviewer may open it on a tablet.

### Guideline 2.3 â€” metadata must match reality

App Store screenshots show features that aren't in the app. The description promises AI features that work differently. The name uses someone else's trademark. The rule is simple: screenshots â€” only of the real interface; description â€” only what actually works.

### Guideline 2.5.2 â€” no executing downloaded code

The app downloads or runs code that changes its behavior after approval. This is why Anything was removed, and why Replit's and Vibecode's updates were blocked. Apps that generate text through an API are not affected â€” generating text is not generating code.

### Guideline 3.1.1 â€” purchases only through Apple's system

Anything sold inside an iOS app â€” only through StoreKit[^28], Apple's payment system. Commission: 15â€“30%.

In the US, as of 2025, you can link to your own website for payment (the outcome of the Epic Games v. Apple case). In the EU, since 2024, alternative payment methods are allowed under the DMA.

Mandatory: the Restore Purchases button must be in a visible location. Subscription price and duration must be clearly visible to the user.

[^28]: **StoreKit** â€” Apple's framework for handling in-app purchases (subscriptions, one-time purchases, consumables). All transactions go through Apple's servers, which take a 15% fee (for the Small Business Program with under $1M/year) or 30%.

### Guideline 3.1.2 â€” subscriptions

Apple mass-rejects so-called "toggle paywalls"[^29] â€” pay screens where the toggle between yearly and weekly subscription defaults to the more expensive option. This is considered a dark pattern[^30].

A subscription screen must show: the exact price, billing frequency (monthly, yearly), length of free trial if any, instructions for canceling, and links to Terms of Use and the Privacy Policy.

[^29]: **Toggle paywall** â€” a payment screen with a toggle between plans. Apple treats it as a violation if the toggle defaults to the most expensive plan, because the user may not notice and pay more than intended.
[^30]: **Dark pattern** â€” a UI design technique that deliberately misleads the user: hidden subscriptions, disguised cancel buttons, pre-selected expensive tiers, and so on.

### Guideline 4.2 â€” minimum functionality

The app is too simple and could just be a website. Wrappers around websites (a WebView[^31] with no native features) get rejected. Not a problem if there are native elements: offline mode, animations, push notifications.

[^31]: **WebView** â€” a component that displays a web page inside a native app. If an app is just a WebView with no native iOS features, Apple considers it insufficiently functional.

### Guideline 4.3 â€” spam and clones

The app is too similar to existing ones in the store. This is one of the trickiest guidelines, because it's subjective. White-label apps[^32] (one codebase, different skins for different clients) â€” instant ban.

Ship one app; don't release clones by category. Keep a unique design. Have clear differences from competitors.

[^32]: **White-label app** â€” the same app published multiple times under different names and branding. Apple treats it as spam.

### Guideline 5.1.1 â€” data collection

The Privacy Policy must be available both on the website and inside the app (usually in settings). App Privacy Labels in App Store Connect must be filled out honestly â€” list every SDK that collects data. A common mistake: forgetting to list Firebase or Amplitude.

### Guideline 5.1.2(i) â€” transferring data to a third-party AI

If the app sends user data to an AI service, you must name the specific recipient (not "service provider" but "Anthropic" or "OpenAI") and obtain explicit user consent before the first transfer.

Example: a screen "Your request will be processed by Claude/GPT to generate content. Continue?"

### Guideline 5.1.2 â€” App Tracking Transparency

If the app uses IDFA[^33] (the device's advertising identifier) â€” for example, for targeted advertising or analytics â€” you must show the standard system ATT[^34] prompt with an explanation of why.

If the app doesn't use advertising or IDFA, this prompt is not required. If there are no ads, you most likely don't need it.

[^33]: **IDFA** (Identifier for Advertisers) â€” Apple's unique device ad identifier. Used by ad networks to track users across apps. Since iOS 14.5 (2021) access requires explicit user consent.
[^34]: **ATT** (App Tracking Transparency) â€” Apple's framework introduced in iOS 14.5. It requires apps to ask users for permission to track. The system dialog "Allow this app to track your activity?" is ATT.

---

## 6. Political and sanctions risks

### China

In 2024 Apple removed WhatsApp, Threads, Telegram, and Signal from the Chinese App Store at the request of the PRC government. China submitted more than 1,300 app removal requests in one year â€” more than any other country in the world.

### Russia

Since 2022, apps removed from the Russian App Store include VK (later partially restored), 25 VPN apps, then another 98 VPN apps at the request of Roskomnadzor. Media apps were removed: "The Insider," BBC Russian, "Echo," "Svoboda."

In February 2025, Apple closed the Apple Developer Enterprise Program for all Russian companies â€” a corporate program for distributing apps inside an organization, bypassing the App Store.

As of April 1, 2026, App Store payments via Russian mobile operators (MTS, Beeline) are discontinued. Users who paid for subscriptions via phone balance lost this payment method.

Takeaway: don't register your developer account to a Russian legal entity. The jurisdiction of registration affects how well the account is protected.

### European Union

In the EU the situation is fundamentally better. The DMA requires Apple to allow alternative app stores, provide developers with detailed explanations for app removals, and allow alternative payment methods. According to 2024â€“2025 data, 30â€“52% of appeals in the EU succeed â€” versus 2.8% in the US.

---

## 7. Apple's review inconsistency

The same app can be approved by one reviewer and rejected by another. This isn't theory â€” real examples:

App approved; a week later an update with minimal changes (a typo fix) is rejected for "trademark in metadata" â€” even though the app name hasn't changed since approval.

An app has been in the App Store for five years, working stably. A bugfix update is rejected with a demand to add in-app purchases â€” even though the app is free and has no paid features.

What to do: if rejected without grounds, file an appeal with the App Review Board. Sometimes just resubmitting without changes helps â€” you'll hit a different reviewer. But don't resubmit many times in a row â€” it's flagged as an attempt to circumvent review (see Case 8 above).

Always save all reviewer correspondence in the Resolution Center â€” it may be needed for an appeal or a lawsuit.

---

## 8. Dark patterns and the FTC

Apple rejects apps for the following design practices: a toggle paywall defaulting to the expensive plan, subscription price in small font or hidden, complex cancellation flow, or an app positioned as free but that does nothing without payment.

The FTC[^35] (US Federal Trade Commission) is investigating Apple for "misleading" marketing of App Store safety â€” Apple promotes the App Store as safe and vetted, but in practice fraudulent apps regularly pass review.

In 2024, the FTC adopted the Click-to-Cancel Rule: canceling a subscription must be as easy as subscribing. If subscribing takes one tap, canceling must also take one tap.

Recommendation: an honest payment screen with the price in large font. A free baseline feature set without a subscription. Cancel subscriptions through Apple's standard interface (device settings).

[^35]: **FTC** (Federal Trade Commission) â€” the US consumer protection and competition regulator. The US equivalent of combining Russia's FAS and Rospotrebnadzor.

---

## 9. Competitor sabotage

Phil Shoemaker, former head of App Store review (November 2025 talk), described this scheme: marketing agencies launch fake ad campaigns aimed at competitors' apps. Thousands of bots download the app, Apple sees anomalous fraudulent traffic, and automatically terminates the victim's account. The victim did nothing â€” they were set up.

His direct quote: "Apple can accuse you of fraud, shut down your account, keep your money, and never tell you why."

How to defend yourself: regularly monitor analytics for anomalous download spikes; use the Apple Search Ads API to track traffic sources. When you spot suspicious activity, preemptively write to Apple Support and build a paper trail[^36], so if you do get blocked you can prove you flagged the problem yourself.

[^36]: **Paper trail** â€” documented confirmation of actions and communications. In the Apple context this means keeping screenshots of correspondence, dates of support tickets, and all emails from Apple.

---

## 10. Mass App Store cleanups

Apple periodically conducts mass removals of apps that: haven't been updated for more than three years, have minimal downloads, or are incompatible with the current iOS version. In 2022, one such cleanup removed more than 186,000 apps.

Recommendation: update your app at least once a year, even if it's just a bugfix. That removes the risk of getting caught in an automatic cleanup.

---

## 11. Developer checklist

### What to do

| # | Action | Why |
|---|---|---|
| 1 | Fill out App Store Connect forms by hand | Apple detects AI generation and terminates the account (Case 2) |
| 2 | One app, not per-category clones | Guideline 4.3 â€” instant ban for spam |
| 3 | Privacy Policy on the website and inside the app | Mandatory, rejection otherwise |
| 4 | AI consent screen with the provider's name | Guideline 5.1.2(i) â€” name Anthropic/OpenAI, get consent before first request |
| 5 | Fill App Privacy Labels honestly | List every SDK that collects data |
| 6 | Restore Purchases button on the subscription screen | Apple checks for it |
| 7 | Subscription price large, period clear, cancel instructions | Dark patterns = rejection |
| 8 | Test account for the reviewer | Full access to all features, including paid |
| 9 | Screenshots of the real UI only | Guideline 2.3 â€” metadata = reality |
| 10 | Legal entity in the EU or UAE | Better legal protection via the DMA |
| 11 | Update the app at least once a year | Otherwise you get swept up in a mass cleanup |
| 12 | Keep all Apple Review correspondence | Needed for appeals |
| 13 | Publish on Google Play in parallel | Backup channel from day one |
| 14 | Web version via Stripe | Backup, not dependent on Apple |
| 15 | User data on your own server | If the app is removed, users aren't lost |
| 16 | When an employee leaves, revoke all access | A former employee can kill the business (Case 9) |
| 17 | Privacy Manifest for every SDK | Mandatory since 2024 |
| 18 | TestFlight beta with 5+ testers | Speeds up first-submission approval |
| 19 | Build with Xcode 26 | Mandatory as of April 28, 2026 |
| 20 | 30â€“60 sec demo video prepared in advance | Automation or reviewer may request it |
| 21 | Account deletion feature | Mandatory since 2022 for apps with registration |
| 22 | Dynamic Type support | Apple checks text scaling |
| 23 | App load under 10 seconds | Longer â€” rejection |
| 24 | Check spelling in the UI | Text errors = rejection |
| 25 | Don't resubmit during the wait | Resets your queue position |
| 26 | Permission prompts in context | Camera/location only when needed, with an explanation |

### What not to do

| # | Don't | Consequences |
|---|---|---|
| 1 | Use AI to fill App Store Connect forms | Account termination (Case 2) |
| 2 | Bot-farm reviews and ratings | Account ban |
| 3 | Resubmit many times in a row | Apple reads it as review circumvention (Case 8) |
| 4 | Ship several similar apps | Guideline 4.3 â€” spam ban |
| 5 | Hide price / toggle paywall on the expensive plan | Rejection + dark pattern |
| 6 | Promise things in the description that aren't there | Guideline 2.3 â€” rejection |
| 7 | Register to a Russian legal entity | Sanctions risk, weak protection |
| 8 | Buy apps without checking history | You inherit the seller's violations (Case 7) |
| 9 | Give dev-account access to outsiders | One person can destroy the business (Case 6) |
| 10 | Activate suspicious gift cards on your Apple ID | Entire Apple ID blocked (Case 4) |
| 11 | Generate executable code inside the app | Guideline 2.5.2 â€” removal |
| 12 | Submit on Friday or before WWDC | Maximum delays, weekend fixes |
| 13 | Forget iPad | Reviewer may check on a tablet |
| 14 | Resubmit while waiting for review | Resets queue position â€” longer wait |
| 15 | Submit the week before WWDC (June 1â€“7, 2026) | Peak queue of the year |
| 16 | Hide or misrepresent AI capabilities | Apple rejects for misleading automation |

---

## 12. If your account is terminated â€” action plan

### First three days

Save every email from Apple verbatim. Take screenshots of App Store Connect while you still have access â€” it will be revoked. File an appeal with the App Review Board (you have 30 days from termination). Don't create a new account right away â€” Apple will link it to the old one via IP, device, and name and terminate it too.

### First two weeks

Hire a California-based attorney specializing in App Store disputes. The attorney sends a demand letter[^37] to Apple's legal department. Write a detailed post about the situation on Medium or your blog. Post it on Hacker News. Contact journalists at TechCrunch, MacRumors, 9to5Mac â€” public pressure remains the most effective tool (see Case 4).

[^37]: **Demand letter** â€” a formal legal letter in which an attorney, on behalf of the client, demands that the other party take a specific action (in this case, restore the account). Often a precursor to a lawsuit.

### In parallel

Google Play keeps working â€” an App Store removal doesn't affect other platforms. The web version via Stripe keeps working. Notify iOS users via push or email about the situation and alternative ways to access the product.

### A year out

File another petition to restore the account. Or create a new legal entity in another country, with different details, a different owner, from a different device and IP address.

### Odds of restoration

| Route | Chance of success | Timeline |
|---|---|---|
| Quiet appeal | ~3% | 5â€“7 business days |
| With press coverage | 30â€“50% | 1â€“4 weeks |
| Attorney + press | 50â€“70% | 2â€“8 weeks |
| Through court | Unpredictable (Case 1 â€” lost) | 6â€“18 months |
| Through the EU process (DMA) | 30â€“52% | 2â€“6 weeks |

---

## 13. Platform diversification

An app should run on three platforms simultaneously:

| Platform | Role | Commission | Note |
|---|---|---|---|
| iOS App Store | Primary channel | 15% (up to $1M/yr) or 30% | App Store Small Business Program |
| Google Play | Alongside iOS | 15% (up to $1M/yr) | Launch from day one |
| Web (Stripe) | Backup channel | 2.9% + $0.30 per transaction | Not dependent on Apple/Google |

Alternative stores have been appearing in the EU since 2024: AltStore PAL, Setapp Mobile, Epic Games Store for iOS. The audience is still small, but growing.

Critically important: store user data (email, profiles, progress, analytics, payment history) on your own server. If Apple removes the app â€” users aren't lost, they can be redirected to the web version or Google Play.

---

## Sources

### Account termination cases
- Seraleev: 8-month investigation â€” medium.com/@seraleev.viktor/my-8-month-investigation-and-court-apple-had-confused-my-developer-account-and-accidentally-killed-53dbde3c6a4c
- Seraleev: Account removed â€” medium.com/@seraleev.viktor/our-developer-account-with-33680-mrr-was-removed-by-apple-a5d1a82ea537
- Indie Hackers: Seraleev rebuilt to $60k â€” indiehackers.com/post/tech/building-an-app-portfolio-to-60k-mo-after-apple-froze-his-developer-account-LD7oNYzKSmWucRfKV1AO
- Reazy: Apple terminated my dyslexia app â€” reazy.pro/blog/apple-terminated-developer-account-no-explanation
- TechCrunch: Appstun WWDC winner â€” techcrunch.com/2024/08/30/apple-stands-by-decision-to-terminate-account-belonging-to-wwdc-student-winner/
- Appstun Termination â€” appstuntermination.com
- MacRumors: Anything AI removed â€” macrumors.com/2026/03/30/apple-pulls-vibe-coding-app/
- MacRumors: Vibe coding apps blocked â€” macrumors.com/2026/03/18/apple-blocks-updates-for-vibe-coding-apps/
- Buttfield-Addison: 20 Years Gone â€” hey.paris/posts/appleid/
- Android Police: Google terminated for former employee â€” androidpolice.com/google-terminate-personal-account-former-employee-violated-policies/

### Statistics and reports
- MacRumors: Apple 2024 Transparency Report â€” macrumors.com/2025/05/30/app-store-2024-transparency-report/
- AppleInsider: Millions denied in 2024 â€” appleinsider.com/articles/25/05/30/millions-of-apps-were-denied-by-apple-in-2024-amid-fraud-crackdown
- Google: 80K accounts banned 2025 â€” androidheadlines.com/2026/02/google-banned-80000-bad-developer-accounts-in-2025.html

### Sabotage and fraud
- MobileGamer: Shoemaker on fraud crackdown â€” mobilegamer.biz/app-store-fraud-crackdown-wrongly-nukes-accounts-and-withholds-millions-from-devs-says-former-apple-exec/
- Game World Observer: Erroneous fraud suspicions â€” gameworldobserver.com/2025/11/25/apple-is-increasingly-blocking-developer-accounts-due-to-erroneous-fraud-suspicions-claims-the-former-head-of-app-store-review

### Official Apple guidelines
- Apple App Review Guidelines â€” developer.apple.com/app-store/review/guidelines/
- Adapty: Toggle paywalls killed â€” adapty.io/blog/your-toggle-paywall-is-about-to-get-rejected/
- 4.3 Spam saga â€” medium.com/@andriygordiychuk/our-4-3-design-spam-saga-33105602d255

### Legal documents
- Buzko Krasnov: Guide to App Store Disputes â€” buzko.legal/content-eng/guide-to-app-store-disputes-for-developers
- Justia: Sarafan v. Apple â€” dockets.justia.com/docket/california/candce/4:2024cv02698/429105

### Censorship and sanctions
- Wikipedia: Censorship by Apple â€” en.wikipedia.org/wiki/Censorship_by_Apple
- Apple Censorship â€” applecensorship.com
- MacRumors: Russian ADEP closed â€” macrumors.com/2025/02/25/apple-developer-enterprise-program-russia/

### Vibe coding and automated review (2026)
- AppleInsider: Vibe coding boosted submissions 84% â€” appleinsider.com/articles/26/04/05/vibe-coding-significantly-boosted-app-store-review-submissions-in-2025
- 9to5Mac: Apple pushing back on vibe coding apps â€” 9to5mac.com/2026/03/18/apple-pushing-back-on-vibe-coding-iphone-apps-developers-say/
- 9to5Mac: Vibe coding marks the end of App Store review â€” 9to5mac.com/2026/03/29/vibe-coding-developers-report-long-app-store-review-queues/
- WinBuzzer: Apple cracks down, 84% surge â€” winbuzzer.com/2026/04/07/vibe-coding-app-store-surge-apple-crackdown-xcxwbn/
- The Information: Apple cracks down on vibe coding apps â€” theinformation.com/articles/apple-cracks-vibe-coding-apps
- 9to5Mac: Ex-Human sues Apple over $500K â€” 9to5mac.com/2026/04/03/developer-behind-controversial-ai-apps-sues-apple-over-app-store-takedowns/
- AppleInsider: Ex-Human arbitrary enforcement â€” appleinsider.com/articles/26/04/03/ai-startup-suing-apple-over-alleged-arbitrary-enforcement-of-app-store-rules
- Apple Developer Forums: Review delays â€” developer.apple.com/forums/thread/817793
- LowCode Agency: iOS review delays March 2026 â€” lowcode.agency/blog/ios-app-review-delays-march-2026
- Apple Developer: Third-party SDK requirements â€” developer.apple.com/support/third-party-SDK-requirements/
- Apple Developer: Xcode 26 mandatory April 2026 â€” developer.apple.com/news/upcoming-requirements/

### Review preparation
- Apple Developer: App Review â€” developer.apple.com/distribute/app-review/
- Apple Developer: App Review Information â€” developer.apple.com/help/app-store-connect/reference/app-review-information/
- Apple Developer: Privacy manifest files â€” developer.apple.com/documentation/bundleresources/privacy-manifest-files
- Apple Developer: Expedited review â€” developer.apple.com/contact/app-store/?topic=expedite
- Apple Developer: Appeal â€” developer.apple.com/app-store/review/appeal/
- AppNatively: Apple is rejecting AI-generated apps â€” appnatively.com/blog/apple-is-rejecting-ai-generated-apps
- Dev.to: Guideline 5.1.2(i) â€” AI data sharing rule â€” dev.to/arshtechpro/apples-guideline-512i-the-ai-data-sharing-rule-that-will-impact-every-ios-developer-1b0p
- Adapty: App Store Review 2026 Checklist â€” adapty.io/blog/how-to-pass-app-store-review/
- RevenueCat: Guide to App Store rejections â€” revenuecat.com/blog/growth/the-ultimate-guide-to-app-store-rejections/
- Passion.io: App Store approval tips â€” passion.io/blog/how-to-get-an-education-app-approved-on-the-apple-app-store
- Median.co: How to appeal rejection â€” median.co/blog/how-to-appeal-to-app-store-review-after-app-store-rejection
- Firebase iOS SDK: ATT rejection issue â€” github.com/firebase/firebase-ios-sdk/issues/7736

### Review delays and the queue (2026)
- Apple Developer Forums: Long delays in App Review â€” developer.apple.com/forums/thread/816911
- Apple Developer Forums: Significantly Delayed App Review â€” developer.apple.com/forums/thread/817793
- Vocal Media: More Developers Report App Store Review Delays â€” vocal.media/journal/more-developers-report-app-store-review-delays-at-apple
- Key Discussions: Apple developer experience is fraying â€” keydiscussions.com/2026/02/26/from-tahoe-bugs-to-long-app-review-wait-times-even-app-processing-delays-the-apple-app-developer-experience-is-fraying/
- Michael Tsai: Mac App Store Review Times Increasing â€” mjtsai.com/blog/2026/03/02/mac-app-store-review-times-increasing/

### Guideline 4.7 and mini apps
- Dev.to: Apple's New Mini App Rules â€” dev.to/arshtechpro/apples-guideline-47-update-what-every-developer-hosting-html5-mini-apps-must-know-90

### Alternative stores and regulation
- Apple Developer: Changes to iOS in Japan â€” developer.apple.com/support/app-distribution-in-japan/
- MacRumors: Japan App Store Gets Alternative Marketplaces â€” macrumors.com/2025/12/17/japan-app-store-feature-updates/
- Apple Newsroom: Changes to iOS in Japan â€” apple.com/newsroom/2025/12/apple-announces-changes-to-ios-in-japan/
- TechCrunch: Alternative app stores in EU â€” techcrunch.com/2026/02/22/move-over-apple-meet-the-alternative-app-stores-available-in-the-eu-and-elsewhere/
- Apple Developer: DMA and apps in the EU â€” developer.apple.com/support/dma-and-apps-in-the-eu/

### WWDC 2026 and updates
- MacRumors: WWDC 2026 Dates â€” macrumors.com/2026/03/23/apple-announces-wwdc-2026-dates/
- TechCrunch: Apple WWDC June 8-12 â€” techcrunch.com/2026/03/23/apple-wwdc-june-8-12-ai-advancements-siri-developers-conference/
- NotebookCheck: Apple April 2026 Hello Developer â€” notebookcheck.net/Apple-s-April-2026-Hello-Developer-highlights-WWDC26-and-App-Store-analytics.1269091.0.html
- Medium: Apple App Store Submission Changes April 2026 â€” medium.com/@thakurneeshu280/apple-app-store-submission-changes-april-2026-5fa8bc265bbe

### Rejection reasons and checklists (2026)
- The App Launchpad: iOS App Store Review Guidelines 2026 â€” theapplaunchpad.com/blog/app-store-review-guidelines
- EIT Biz: Top Reasons iOS Apps Get Rejected 2026 â€” eitbiz.com/blog/top-reasons-ios-apps-get-rejected-by-the-app-store-and-fixes/
- CrustLab: iOS App Store Review Guidelines 2026 Best Practices â€” crustlab.com/blog/ios-app-store-review-guidelines/
- ASO World: AI-Driven App Explosion 2026 â€” asoworld.com/en/blog/ai-coding-boom-drives-30-surge-in-app-submissions-why-app-growth-is-getting-harder-in-2026/
