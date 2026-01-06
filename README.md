# Web3 Security Notes

Curated collection of smart contract vulnerabilities, exploits, and security best practices with a focus on Solana. This living document tracks my learning journey into blockchain security.

## 🎯 Motivation
As I transition into Web3 security, I'm documenting:
- Common vulnerability patterns
- Real-world exploit analyses
- Prevention techniques
- Tooling and resources

## 📚 Contents

### 1. [Solana-Specific Vulnerabilities](/solana/)
- PDA derivation errors
- Account confusion attacks
- CPI (Cross-Program Invocation) risks
- Reentrancy on Solana

### 2. [Rust Security Patterns](/rust/)
- Unsafe usage guidelines
- Integer overflow prevention
- Error handling best practices

### 3. [Audit Checklists](/checklists/)
- Pre-deployment review items
- Testing strategies
- Monitoring and incident response

### 4. [Exploit Case Studies](/case-studies/)
- Analysis of historical hacks
- Root cause breakdowns
- Lessons learned

## 🔗 Key Resources
- [Solana Sealevel Attacks](https://github.com/solana-labs/sealevel-attacks)
- [Awesome Solana Security](https://github.com/0xNezha/awesome-solana-security)
- [Smart Contract Weakness Classification](https://swcregistry.io/)
- [Neodyme Security Blog](https://www.neodyme.io/blog/)

## 🛠️ How to Use This Repo
```bash
# Clone and explore
git clone https://github.com/YOUR_USERNAME/web3-security-notes.git

# The notes are organized by topic in markdown files
# Start with /solana/pda-security.md for Solana-specific content

🚀 Ongoing Learning
This repository is actively updated as I:

Complete security courses

Analyze new exploits

Build and audit practice contracts

Participate in security communities

Maintained as part of my application to the Rektoff & Solana Foundation Rust Security Bootcamp.


**File: `solana/pda-security.md`**

```markdown
# PDA (Program Derived Address) Security

## What are PDAs?
Program Derived Addresses are addresses that don't lie on the Ed25519 curve, meaning they have no corresponding private key. They are controlled by programs.

## Common Vulnerabilities

### 1. Incorrect PDA Derivation
**Issue:** Using wrong seeds or program ID
```rust
// WRONG - Missing bump seed
let (pda, bump) = Pubkey::find_program_address(&[b"account"], program_id);

// WRONG - Using wrong seeds
let (pda, bump) = Pubkey::find_program_address(&[b"wrong_seed"], program_id);

Fix: Always use find_program_address and include the bump in validation.

2. Missing PDA Validation
Issue: Not verifying a PDA was derived correctly
#[derive(Accounts)]
pub struct UpdateAccount<'info> {
    #[account(mut)]
    pub pda_account: AccountInfo<'info>, // Missing validation!
}

Fix: Use Anchor's seeds and bump constraints

#[derive(Accounts)]
pub struct UpdateAccount<'info> {
    #[account(
        mut,
        seeds = [b"account", user.key().as_ref()],
        bump = pda_bump
    )]
    pub pda_account: Account<'info, MyAccount>,
}

Best Practices
Always store the bump seed in account data

Validate PDA in client before sending transaction

Use Anchor's constraints when possible

Test edge cases: empty seeds, max length seeds

Real-World Example
The Wormhole exploit (Feb 2022, $326M) involved PDA validation issues among other vulnerabilities.


**Commit these to your third repo.**

---

### **Step 5: Update Your GitHub Profile**

Add this to your **GitHub profile README** (create `.github/README.md` in your profile repo):

```markdown
## 🛡️ Security-Focused Developer | Solana & Rust

Transitioning from backend development to blockchain security, with a focus on Solana smart contract auditing and secure protocol design.

### 🔭 Currently
- Preparing for the Rektoff & Solana Foundation Rust Security Bootcamp
- Building educational repos for Solana security
- Deep diving into Rust's ownership model and memory safety

### 📚 Learning Repositories
- [Solana Security Basics](https://github.com/YOUR_USERNAME/solana-security-basics) - Anchor programs with security notes
- [Rust Memory Safety Demos](https://github.com/YOUR_USERNAME/rust-memory-safety-demos) - Ownership & borrowing examples
- [Web3 Security Notes](https://github.com/YOUR_USERNAME/web3-security-notes) - Vulnerability research & best practices

### 🎯 Goals 2026
1. Complete advanced Solana security training
2. Contribute to open-source security tooling
3. Perform my first independent smart contract audit
4. Specialize in DeFi protocol security

### 📫 Connect
- Twitter: [@yourhandle](https://twitter.com/yourhandle)
- LinkedIn: [Your Name](https://linkedin.com/in/yourprofile)
- Telegram: @yourhandle

---
*"Security is not a product, but a process."* - Bruce Schneier
