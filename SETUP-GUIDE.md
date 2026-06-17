# Setting up measuremodellead.co.uk

This guide assumes no command line and no prior GitHub experience. Everything
happens through GitHub's website and GoDaddy's website. It's written in five
parts: what you're actually building, creating the repository, uploading the
files, turning on GitHub Pages, and pointing the domain at it.

A couple of terms used throughout, explained once so the rest reads cleanly:

- **Repository** (or "repo") is GitHub's word for a project folder. Yours
  will hold every file that makes up the website.
- **Jekyll** is the tool that turns the plain text files in this project into
  an actual website. You will not need to install it or run it yourself.
  GitHub has it built in, and builds your site automatically every time you
  upload or change a file.
- **DNS** is the system that tells the internet which server to fetch a
  website from when someone types your domain into a browser. You'll edit
  this in GoDaddy, since that's where the domain is registered.

## Part 1: what you're building

GitHub Pages is a free hosting service. You put your website's files in a
GitHub repository, switch on a setting called Pages, and GitHub serves the
site at a free address like `yourusername.github.io`. Because you already own
measuremodellead.co.uk through GoDaddy, the last step points that domain at
GitHub's address instead, so visitors see your own domain rather than
GitHub's.

There's no server to manage and nothing to pay for. The only ongoing cost is
the domain renewal you already have with GoDaddy.

## Part 2: creating the GitHub repository

1. Go to [github.com](https://github.com) and sign in, or create a free
   account if you don't already have one.
2. Click the **+** icon in the top right corner, then **New repository**.
3. Name it something like `mml-website`. The name doesn't affect your final
   domain, since you'll be pointing your own domain at it either way.
4. Set it to **Public**. Public repositories get free GitHub Pages hosting;
   private ones need a paid plan to use Pages.
5. Leave "Add a README file" unticked, since this project already includes
   one.
6. Click **Create repository**.

You'll land on an empty repository page with an upload option front and
centre.

## Part 3: uploading the files

GitHub's web uploader supports dragging an entire folder in, and it will
keep the folder structure intact. This matters here, since several folders
in this project start with an underscore, like `_layouts` and `_posts`. That
underscore is just Jekyll's naming convention, not a hidden file, so it will
upload normally in any modern browser (Chrome, Edge, and current Safari all
handle this correctly).

1. On your new repository's page, click **uploading an existing file** (or
   **Add file → Upload files** if you don't see that link).
2. Open the folder on your computer that contains this project (it should be
   named `mml-site` or similar after unzipping the file you downloaded).
3. Select everything inside that folder, including all the subfolders, and
   drag it into the browser window onto the upload area. Don't drag the
   outer `mml-site` folder itself, drag what's inside it, so the files land
   at the top level of the repository rather than nested inside an extra
   folder.
4. Wait for the upload list to finish populating. You should see `_config.yml`,
   `_layouts`, `_includes`, `_posts`, `assets`, `offers`, `blog`, `index.md`,
   `about.md`, `white-paper.md`, `CNAME`, `README.md`, and `SETUP-GUIDE.md`
   all listed.
5. Scroll down, add a short commit message like "Initial upload", and click
   **Commit changes**.

If your browser won't accept a folder drag, the fallback is uploading file by
file and recreating folders as you go, using **Add file → Create new file**
and typing a path like `_posts/2026-06-01-sample-post-one.md` into the
filename box, which makes GitHub create the folder automatically. This is
slower but works in any browser.

## Part 4: turning on GitHub Pages

1. In your repository, click **Settings** (top right of the repository's own
   menu bar, not your account settings).
2. In the left sidebar, click **Pages**.
3. Under "Build and deployment", set **Source** to **Deploy from a branch**.
4. Under "Branch", choose **main** and **/ (root)**, then click **Save**.
5. GitHub will take a minute or two to build the site for the first time.
   Refresh the Pages settings page until you see a message confirming your
   site is published, along with a link to a `github.io` address.
6. Visit that link and check the site loads. This confirms the build worked
   before you touch any DNS settings.
7. Still on the Pages settings page, find **Custom domain**, type in
   `measuremodellead.co.uk`, and click **Save**. GitHub will show a "DNS
   check in progress" or similar warning at this point. That's expected,
   since you haven't updated GoDaddy yet, which is the next part.
8. Leave this page open or come back to it after Part 5. Once the DNS check
   passes, a checkbox for **Enforce HTTPS** will become available. Tick it,
   since this makes the site load securely over `https://` rather than
   `http://`.

## Part 5: pointing measuremodellead.co.uk at GitHub from GoDaddy

GitHub Pages is hosted on a small set of fixed IP addresses. To point your
domain at it, you add four **A records** for the bare domain
(measuremodellead.co.uk) and one **CNAME record** for the `www` version.

1. Log into [godaddy.com](https://godaddy.com) and go to **My Products**.
2. Find measuremodellead.co.uk and click **DNS** (sometimes labelled
   **Manage DNS**).
3. Look through the existing records. If you see any A records or CNAME
   records already pointing at something else, e.g. a parked page, a
   previous Netlify deployment, or GoDaddy's own placeholder page, delete or
   edit those, since conflicting records are the most common reason a domain
   doesn't resolve correctly afterwards.
4. Add four A records, all with the **Name/Host** set to `@` (GoDaddy's way
   of meaning "the bare domain"), each pointing to one of these addresses:

   | Type | Name | Value           | TTL     |
   |------|------|-----------------|---------|
   | A    | @    | 185.199.108.153 | 1 hour  |
   | A    | @    | 185.199.109.153 | 1 hour  |
   | A    | @    | 185.199.110.153 | 1 hour  |
   | A    | @    | 185.199.111.153 | 1 hour  |

   These four addresses belong to GitHub and don't change based on your
   username or repository, so you can copy them exactly as shown.

5. Add one CNAME record so the `www` version of your site also works and
   redirects to the main domain:

   | Type  | Name | Value                          | TTL    |
   |-------|------|---------------------------------|--------|
   | CNAME | www  | [YOUR-GITHUB-USERNAME].github.io | 1 hour |

   Replace `[YOUR-GITHUB-USERNAME]` with your actual GitHub username, the
   one in the `github.io` address you confirmed in Part 4, step 6.

6. Save the changes. GoDaddy may ask you to confirm each record individually
   or all at once, depending on its current layout.
7. If GoDaddy shows a separate "Forwarding" section for this domain and it's
   currently forwarding to anything (a parked page, a previous site), turn
   that off. Forwarding and DNS-based hosting conflict with each other.

DNS changes can take anywhere from a few minutes to a few hours to take
effect everywhere, occasionally up to 24 to 48 hours, though it's usually
much faster. Go back to the GitHub Pages settings page from Part 4 and wait
for the DNS check to turn green, then tick **Enforce HTTPS**.

Once that's done, measuremodellead.co.uk should load your site directly,
with `www.measuremodellead.co.uk` redirecting to it.

One last thing worth flagging: if the previous vibe-coded version of this
site is still live on Netlify, it won't be affected by anything above, since
it's a separate deployment on different infrastructure. Once you've
confirmed the GitHub Pages version is working correctly on your own domain,
you can go back into Netlify and take that deployment down, so there's only
one live version of the site going forward.

## Updating the site later

Once everything is live, adding a new blog post or changing a page works the
same way: go to the relevant file in the repository on GitHub's website,
click the pencil icon to edit it directly in the browser, make your change,
and commit it. GitHub rebuilds the live site automatically within a minute
or two of any change, no separate publish step required.

To add a brand new blog post, use **Add file → Create new file** inside the
`_posts` folder, name it in the format `YYYY-MM-DD-a-short-title.md`
(matching the existing sample posts), and copy the front matter pattern from
one of the samples at the top of the file.
