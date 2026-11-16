# My portfolio website: how to update it

The site is built by GitHub Pages with Jekyll. You never edit the design. You only add or change content files,
and GitHub rebuilds the site in 1-2 minutes after each change.

## Change my details (CV, email, summary)
Open `_config.yml`.
- **CV:** upload your PDF into `assets/files/`, then set `cv_file: /assets/files/YOUR-FILE-NAME.pdf`.
  The "Download CV" buttons appear automatically.
- **Email:** write it between the quotes in `email: ""`. Leave it empty to hide it.

## Add a certificate
Open `_data/certifications.yml`. Copy one block (from `- name:` to the next blank line), paste it, and change the values.
- `status: earned` shows a solid card; `studying` or `planned` shows a dashed "working towards" card.
- When you pass one, change its `status` to `earned` and add `year:` and `verify_url:`.

## Add a project
1. In the `_projects` folder, copy `harbourline.md` and rename the copy, for example `sandbox-deployment.md`
   (the file name becomes the web address: `/projects/sandbox-deployment/`).
2. Change the lines at the top between the two `---` lines: title, summary, status_label, tags, role, dates, `order`
   (1 = first) and `featured: true` if it should show on the Home page (the Home page shows up to 3).
3. Write the case study under the second `---`, using the same 8 sections.
4. Put screenshots in `assets/img/your-project-name/` and use them like the Harbourline ones.
The Projects page and the Home page list the new project automatically.

## Change experience, skills or education
Edit `_data/experience.yml` or `_data/profile.yml`. Keep them the same as your CV.

## Rules that keep it working
- In `.yml` files, the spaces at the start of each line matter. Copy an existing block and keep the same indentation.
- Don't use the Tab key in `.yml` files; use spaces.
- If the site doesn't update, open the repository's **Actions** tab. A red cross shows which file has a problem.
