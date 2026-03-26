# ⚔️ CTF vs Professional Engagement

CTF (Capture the Flag) competitions and real professional penetration tests both use the same tools — but the rules, pace, and consequences are completely different.

Think of it like cooking at home versus cooking in a restaurant kitchen. Same knives, same techniques — but in a restaurant, speed matters, presentation matters, and mistakes have real consequences.

---

## 🔑 The Core Difference

In a CTF, the goal is to find the flag and win. In a professional engagement, the goal is to find weaknesses, document everything, and help the client fix them — without causing damage or getting caught.

---

## 📊 Side by Side Comparison

| Topic | CTF | Professional Engagement |
|---|---|---|
| **Permission** | Implied by the competition | Written and signed before you touch anything |
| **Scope** | The whole box is fair game | Strictly defined — only scan what's approved |
| **Speed** | Faster is better | Slow and deliberate — T2 or T3 timing |
| **Noise** | Doesn't matter | Matters — stay under IDS/IPS radar |
| **Full port scan** | Always run it | Only if it's in scope and approved |
| **Aggressive scan (-A)** | Go for it | Avoid — too loud, can crash services |
| **Destructive actions** | Sometimes required for the flag | Never — you could take down production systems |
| **Output/Logging** | Good habit | Required — everything goes in the report |
| **Metasploit** | Use freely | Check rules of engagement first |
| **Scripts (--script)** | Use freely | Verify they won't cause damage first |
| **Social engineering** | Rarely in scope | Sometimes in scope — must be explicitly approved |
| **Cleanup** | Not required | Required — remove all tools and backdoors |

---

## 🧠 Mindset Shift

### In a CTF you think:
> *"How do I get in fastest?"*

### In a professional engagement you think:
> *"How do I demonstrate risk without causing harm, and how do I document it so clearly that the client can fix it?"*

---

## 📋 Before Every Real Engagement — Checklist

- [ ] Signed Rules of Engagement (ROE) in hand
- [ ] Scope document reviewed — know exactly what IP ranges and domains are approved
- [ ] Emergency contact for the client in case something breaks
- [ ] Your own legal counsel has reviewed the contract
- [ ] Start time and end time agreed upon
- [ ] Output logging enabled on every tool

---

## 🚨 The Golden Rule

**If it's not in writing, don't touch it.**

Unauthorized access to computer systems is a federal crime in the United States under the Computer Fraud and Abuse Act (CFAA) and equivalent laws exist in most countries. A CTF teaches you the skills. The rules of engagement keep you out of prison.

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*