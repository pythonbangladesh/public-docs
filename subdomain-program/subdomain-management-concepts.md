# Subdomain Management Concepts

A subdomain is a prefix added to a domain name. It creates separate sections within a website. 
These sections can be managed independently. This allows for organization of content like blogs, events, or support platforms. 
Subdomains act as separate websites linked to the main domain. They enable focused management and unique functionalities.

This guide covers the different approaches to subdomain management available through the Python BD subdomain program.

---

## Example Scenario

Python BD controls [pythonbd.org](https://pythonbd.org), which is hosted on Cloudflare. 
We want to provide [khulna.pythonbd.org](https://khulna.pythonbd.org) subdomain for our divisional user group Python User Group Khulna. 

We offer three management approaches:

**1. Direct Subdomain Pointing** - We point the subdomain (e.g., `prfix.pythonbd.org`) to your hosting service (e.g., `aimlcommunity.github.io`, `pybdsite.onrender.com`)  
**2. Subdomain Aliasing** - We create an alias to your existing domain (e.g., `pyladiesbd.com`, `datanurd.site`)  
**3. Subdomain Delegation** - We transfer complete DNS control to your nameserver

---

## Management Methods Explained

---

### 1. Direct Subdomain Pointing

This is the simplest approach. 
Python BD uses `A`, `AAAA`, or `CNAME` records to point `khulna.pythonbd.org` directly to the Khulna User Group's server. 
Python BD maintains full DNS control over the subdomain. Traffic gets directed to the group's infrastructure.

This method works perfectly when the local group doesn't own their own domain but has hosting capabilities. 
Free hosting services like Render or GitHub Pages also works with it.

#### Implementation Steps

**1. Set Up Hosting**: The Khulna team creates a static/dynamic website and hosts it somewhere. Some example can be:  
   - Deploy a Flask application on Render.com as a web service (completely free)
   - Can Use other platforms like Heroku, AWS, or any server
   - Create a static site with HTML/CSS and host it on GitHub Pages or Render

**2. Obtain Host Address**: The Khulna team provides the IP address or default subdomain from their hosting provider:
   - Examples: `khulna.github.io`, `khulna.onrender.com`

**3. Configure DNS**: Python BD points the custom subdomain `khulna.pythonbd.org` to the provided default subdomain or IP address.

**4. Enable Custom Domain**: The Khulna adds `khulna.pythonbd.org` as a custom domain on their hosting service.

#### Advantages
- Simplest setup process
- No cost involved for the user group
- Python BD maintains DNS control and security from couldflare

#### Drawbacks
- If the user group changes hosting providers, the host domain/IP will change. This requires DNS updates from Python BD
- Some static site hosts (GitHub Pages, Vercel) is problematic with custom domains

---

### 2. Subdomain Aliasing

This approach applies when the user group already owns a domain name (e.g., `pykhulna.org`). 
Python BD creates a `CNAME` record that makes `khulna.pythonbd.org` an alias for the group's existing domain. 
This mirrors the group's site under the pythonbd.org subdomain structure.

The user group simply adds the subdomain `khulna.pythonbd.org` to their website's allowed hosts or custom domains configuration.

#### Advantages
- Simple setup requires only a `CNAME` record
- Dicoverable with both domains (`pykhulna.org` and `khulna.pythonbd.org`)
- Enhances authority association with Python BD

#### Disadvantages
- May have compatibility issues if the target domain also uses Cloudflare nameservers
- The user group bears the cost of domain registration and maintenance

---

### 3. Subdomain Delegation

Subdomain delegation transfers complete DNS authority for a subdomain to different nameservers. 
This approach creates `NS` (nameserver) records that delegate full DNS management of `khulna.pythonbd.org` to the user group's DNS servers.

This method gives the user group complete control to create any DNS records they want under their subdomain. 
Examples include `events.khulna.pythonbd.org` or `blog.khulna.pythonbd.org`.

#### Example Use Case
For PyCon Bangladesh, we might delegate `pycon.pythonbd.org` to the organizing team. They gain full control to create subdomains like:
- `events.pycon.pythonbd.org` for event management
- `2026.pycon.pythonbd.org` for PyCon BD 2026 information

#### Technical Implementation

Since [pythonbd.org](pythonbd.org) uses Cloudflare, subdomain delegation to another Cloudflare account isn't possible. 
However, delegation to different nameservers (like NameCheap) is supported.

**Steps:**

**1. NameCheap Account**: Register/Login to your NameCheap account

**2. Access FreeDNS Service**: Visit **NameCheap's FreeDNS** service at [https://www.namecheap.com/domains/freedns/](https://www.namecheap.com/domains/freedns/)

**3. Add Subdomain**: Search for and add `pycon.pythonbd.org` to set up DNS management
<img width="1865" height="1008" alt="Screenshot from 2025-08-04 04-28-37" src="https://github.com/user-attachments/assets/f1314b64-51e2-444b-81f4-dc69ac965760" />

**4. Obtain Nameservers**: You'll receive 4-5 nameserver addresses like below. Save them somewhere for now.
   ```
   freedns1.registrar-servers.com
   freedns2.registrar-servers.com
   freedns3.registrar-servers.com
   freedns4.registrar-servers.com
   freedns5.registrar-servers.com
   ```

**5. Authorize FreeDNS**: 
   - Go to your NameCheap dashboard
   - Authorize FreeDNS for `pycon.pythonbd.org`
   - Copy the `TXT` record value from Host Records
   - Select `admin@pythonbd.org` for email verification (NameCheap will send a verification link to `admin@pythonbd.org`)
  <img width="1865" height="1008" alt="Screenshot from 2025-08-04 04-42-20" src="https://github.com/user-attachments/assets/d50ab84f-07d1-4bba-8f71-102bd535ce45" />


**6. Provide Records to Python BD**: Send both the `NS` and `TXT` records to the Python BD team

**7. Activation**: After Python BD adds these records and verify email, activation takes approximately 30 minutes

**8. Configure Your Services**: Add `CNAME` or `A` records to use `pycon.pythonbd.org` as a custom domain for your hosting services

#### Advantages
- Complete DNS management control for the subdomain
- Inherits SSL certificate from the parent domain (pythonbd.org)
- Maximum flexibility for complex DNS requirements

#### Drawbacks
- Cannot use Cloudflare's free tier due to existing [pythonbd.org](pythonbd.org) configuration (should use a different nameserver)
- The delegated subdomain no longer benefits from Cloudflare's security features (WAF, DDoS protection, bot prevention) that protect `pythonbd.org`
- Complex setup and not recommended for DNS management beginners
- Creates multiple DNS records in Python BD's dashboard (5 `NS` + 1 `TXT` record per subdomain) which looks messy

---

## Subdomain Program Guidelines

---

### Recommended Approaches by Use Case

**1. Direct Subdomain Pointing** - Recommended for:
- **Divisional User Groups**: `khulna.pythonbd.org`, `chittagong.pythonbd.org`, `sylhet.pythonbd.org`
- **University Chapters**: `du.pythonbd.org`, `sust.pythonbd.org`, `buet.pythonbd.org`
- **Allied Communities**: `aimlcommunity.pythonbd.org`, `datanurd.pythonbd.org`

**2. Subdomain Delegation** - Reserved for series of major or collaborative initiatives:
- **Conferences**: `pycon.pythonbd.org`, `djangocon.pythonbd.org`
- **Collaborative Events**: `datathon.pythonbd.org`

**3. Subdomain Aliasing** - Available when:
- You already own a domain
- Compatibility with Cloudflare nameservers is confirmed through testing

### Security Considerations

Subdomain delegation transfers complete DNS control to external parties. 
This includes the ability to create wildcard records within the delegated zone. 
Therefore, subdomain delegation is carefully evaluated and approved only for established, trusted body with legitimate complex DNS requirements.

---

## Getting Started

---

If you're interested in obtaining a subdomain through the Python BD program, 
please submit an application to [admin@pythonbd.org](mailto:admin@pythonbd.org) with:

- Your organization/group information (name, description) 
- Email addresses of 2 people as primary contacts  
- Intended use case  
- Preferred subdomain name (e.g., `ruet.pythonbd.org`, `aimlcommunity.pythonbd.org`)  
- Concise technical requirements

### Prerequisites

- You already have a website up and running  
- Your website has contains **Activities**, **Team**, and **Code of Conduct** sections  
- Your **Code of Conduct** aligns with the **[Python BD Code of Conduct](https://github.com/pythonbangladesh/public-docs/blob/main/code-of-conduct.md)**  

We'll review your application and help determine the most appropriate management method for your specific needs.
