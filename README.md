# The Mellis Lab website

Our website, [www.mellislab.org](http://mellislab.github.io) is a [GitHub Pages](https://pages.github.com/) site built with [Jekyll](https://jekyllrb.com/) (a static site generator written in ruby) and [Bootstrap](http://getboostrap.com).  The software framework was forked from [www.getzlab.org](http://www.getzlab.org), which was built from a fork from [Alan Drummond's site](http://drummondlab.org), which was originally pulled from [Trevor Bedford's site](http://bedford.io) and modified.

Content and presentation are separated.  To add content to the site, one simply needs to update or add new markdown files, commit the changes to this repo's master branch and those changes will automatically be deployed to the public site.

We chose this platform for the lab's website to enable "crowdsourcing" of the website's content: lab members will be responsible for maintaining their own member webpages and publication authors will be responsible for adding links to their publications on the site's Papers pages.  A handful of lab members will be responsible for maintaining the Tools, Portals and Join (i.e., job listings) pages.

# Editing the site

Here's a step-by-step guide for making modifications to the site. We will first focus on adding the most common content (member pages and paper pages).  We will then discuss creating/updating the remaining content categories.

## Software Requirements

You'll need a working Unix-like environment with Git installed, and working knowledge of [Git](https://git-scm.com/), [Markdown](https://daringfireball.net/projects/markdown/syntax), HTML, and Unix commands. 

Git comes pre-installed on most recent versions of MacOS.  See [git-scm-installation](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) for instructions on how to determine whether you have git already installed, how to install it if you don't, and how to upgrade to the latest version.  If you wish to use the GitHub desktop application rather than the CLI, see [GitHub Desktop](https://desktop.github.com/) for installation instructions.

To locally preview your content updates, you will need to run a local instance of Jekyll to generate the static files and serve them locally.  Locally previewing files is most easily done by running Jekyll within a Docker container and serving the website locally from that container, which will require the installation of [Docker](https://www.docker.com/). **Running Jekyll within a docker container is how we recommend lab members locally test their contributions to the website.**  [Previewing your local edits](#previewing-your-local-edits) contains instructions for installing docker on your development system and serving your version of the website locally from a running jekyll container.

Alternatively, you may install Ruby, with gems for Jekyll, GitHub Pages, and their dependencies, directly on your development system. (If you choose to go this route, you will not need to install Docker.)  The installation of these tools can be a little confusing, depending on your hardware (intel vs. M1 chip) the OS environment (Catalina vs. Big Sur) and the shell you are using (bash vs. zsh)  I recently needed to upgrade to Big Sur (required by BITS) and the upgrade broke my ruby/jekyll dev environment.  I did get it reassembled and working again, but it took a couple of hours.  See the following instruction: 


[Jekyll Installation on Mac](https://jekyllrb.com/docs/installation/macos/)


## Cloning the Getz Lab website's GitHub Repository

The website's source code and documentation (i.e., this README file) are located in the GitHub repository [https://github.com/mellislab/mellislab.github.io](https://github.com/mellislab/mellislab.github.io).  This is a public repo, so anyone should be able to clone the repo.  However, in order to publish your changes back to the GitHub repo and issue pull requests, you will need write access to the repository.  Members of the [all_mellislab team](https://github.com/orgs/mellislab/teams/all_mellislab) within the mellislab GitHub organization have write access to the repo.

When accessing GitHub via the git command line interface (CLI) to publish your local changes to the repo, you will need to authenticate yourself to GitHub.  How you authenticate to GitHub when using the CLI depends on the protocol you use to communicate with GitHub.  The options are https or SSH.  You specify the protocol when you clone the repo.  If you clone the repo by issuing the following git command:

    git clone https://github.com/mellislab/mellislab.github.io
    
you will be using the https protocol for which the required authentication protocol is Personal Access Token (PAT).  Read [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) for instructions on how to create and use PAT.

If you clone the repo by issuing the following git command:

    git clone git@github.com:mellislab/mellislab.github.io.git

you will be using the SSH protocol to connect to github.  Before issuing this clone command you will need to ensure you have an SSH key installed on your laptop, added to the ssh-agent and added to your GitHub account.  Instructions for doing this are [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

If you specified the https protocol when you cloned the repo but want to switch to SSH, issue the following git command:

    git remote set-url origin git@github.com:mellislab/mellislab.github.io

You must have an SSH key added to your GitHub account before issuing the above command.

## Overview of the structure

A site is a collection of HTML pages. For our site (and many others), there are pages of the same type, like paper (publication) pages or a lab member pages, which have the same layout but differ in content. In the web-accessible site, these are indeed different pages. However, they are _generated_ from a single template file filled in with information from many paper- or member-specific markdown data files. This generation is done every time the site changes; it's handled by GitHub Pages, the service we use.

The template files are weird-looking HTML files (containing jekyll site variables and control logic) residing in the `_includes/themes/lab` folder.  You should not alter the contents of this folder.

## How to add content

You will create a personal branch off of master, make your changes on that branch, preview your changes on a locally-served instance of the website and, when satisfied with the appearance of your changes, push your local branch to the github repo:

	git checkout -b <your name>-staging

	...

	git push -u origin <your name>-staging

When you create your branch with the checkout command, make sure you have the master branch checked out.  This will ensure your branch is created off of master.  After pushing your personal staging branch to github, create a pull request for merging your changes into the **master** branch and request a review.  The reviewer (cbirger for now) will be responsible for merging your updates to master, which publishes the updated content to the public website.  I discuss this process of creating a pull request in greater detail in [Updating the public site](#updating-the-public-site).

### Previewing your local edits

You should preview your local edits by having a locally running Jekyll installation generate the static pages and serve them for review.  You may run Jekyll directly on your development system (requiring the local installation of Ruby and the Jekyll and GitHub Pages Gems) or, more simply, run Jekyll within a Docker Container.  

Before running Jekyll from within a Docker container, you will need to install docker on your system and run the docker daemon (background service).  Installation of docker is most easily done by installing the [Docker Desktop](https://www.docker.com/get-started).  After installing Docker Desktop and launching the app, he docker daemon should be running.

To run Jekyll from within a Docker container simply issue the following command:

	$ docker run --rm --volume="$PWD:/srv/jekyll" -p 4000:4000 jekyll/jekyll:4.0 jekyll serve
	
where the current working directory is the top-most directory of the cloned mellislab.github.io repository.

If you chose to install Ruby and Jekyll on your development system, the following command, run from the top-most directory of the cloned repo, will generate and serve the content for preview:

	$ rake preview

[Rake](https://github.com/ruby/rake) is a Make-like program implemented in ruby and part of the ruby/jekyll environment on which our website is built.

In either case, open the local test site, http://127.0.0.1:4000, from your browser. Look at anything you've changed and make sure it's good to go.  While the server is running (either directly on your computer or within a Docker container), any file updates will be automatically detected and impacted static files regenerated.

### Example workflow

For the most common activities --- adding/updating a lab member page or adding a publication page --- you'll be making a new, or updating an existing, markdown file. The markdown files contain a YAML front matter block which is processed by Jekyll.  Different categories of website content (e.g., paper, member, portal) have different YAML dictionary keys in their front matter, so in almost all cases you can (and should!) copy an existing item, change the name, and change its content rather than trying to write a Markdown document from scratch.

For example, suppose you recently had a paper published and want it listed on the Getz Lab website.  Creating a new paper "post" in the `papers/_posts` folder will add your new publication to the website's publications listing.  Go into the `papers/_posts` folder. Copy one of the existing items into a new file whose name is prefaced with the paper's publication date (this is important!) and an abbreviated version of the title.  For example, we want to add a recent (5/1/2021) Cancer Discovery paper co-authored by a large team of researchers where Gaddy is a co-senior author and Yosi Maruvka is a co-first author.  The paper's title is "DNA Polymerase and Mismatch Repair Exert Distinct Microsatellite Instability Signatures in Normal and Malignant Human Cells".  First thing we do is cd to the papers/_posts subdirectory and, using a recent paper post as a template, create a file for our new publication:

	cp 2021-04-22-rebc-genomic-profile.md 2021-05-01-mismatch-repair-msi-signatures.md

The date which prefaces the filename is used by the static file generator; it's inelegant and perhaps there's a way to do it differently, but that's how it is for now. Now edit the new file to make the content what you want. Just open it in your favorite editor and type away. By the time you're done, hopefully you have something like this:

		---
		layout: paper
		title: "Antibody evasion and receptor binding of SARS-CoV-2 LP.8.1.1, NB.1.8.1, XFG, and related subvariants"
		journal: Cell Reports
		volume: 44
		issue: 10
		pages: 116440
		year: "2025"
		authors: Mellis IA, Wu M,  Hong H, Tzang CC, Bowen A, Wang Q, Gherasim C, Pierce VM, Shah JG, Purpura LJ, Yin MT, Gordon A, Guo Y, Ho DD 
		front_authors: Mellis IA, Wu M, Hong H, Tzang CC, Bowen A
		first_authors: Mellis IA, Wu M, Hong H, Tzang CC, Bowen A
		senior_authors: Gordon A, Guo Y, Ho DD
		back_authors: Gordon A, Guo Y, Ho DD
		fulltext:
		doi: 10.1016/j.celrep.2025.116440
		pmid: 41091599
		category: paper
		---

		# Abstract

		SARS-CoV-2 continues to evolve, causing waves of infections. It is critical to understand the features of the virus that explain its growth advantages. Recently, SARS-CoV-2 Omicron JN.1 subvariants KP.3.1.1 and XEC were outcompeted by LP.8.1 and LP.8.1.1. Other subvariants, including LF.7.2.1 and MC.10.1, were also under monitoring. Subsequently, NB.1.8.1 and XFG became dominant. We found that serum neutralizing antibody titers against LP.8.1, LP.8.1.1, LF.7, LF.7.2.1, and MC.10.1 were similar to XEC in 40 adults, including KP.2 monovalent mRNA vaccine recipients. NB.1.8.1 and XFG were more evasive of serum neutralization than LP.8.1.1. Neutralization by 12 monoclonal antibodies (mAbs) revealed that LP.8.1 and XFG, MC.10.1 and NB.1.8.1, and LF.7.2.1 evade different mAb classes. Lastly, the receptor-binding affinity of LP.8.1 was the highest among the tested viruses. Unlike most prior SARS-CoV-2 sublineage evolutionary trajectories, receptor-binding affinity better explained the rise of LP.8.1, while expansion of NB.1.8.1 and XFG appears correlated with enhanced antibody evasion.

Run a dockerized version of Jekyll to preview your updates (see [Previewing your local edits](#previewing-your-local-edits)).  Once you are satisfied with the updated content, add it to the repository, commit, and push to GitHub:

	git add 2025-10-14-LP81.md
	git commit -m "Ian's LP81 paper"
	git push origin <your name>-staging

This new paper won't yet be public. The next section shows you how to do that.

The same basic process is used to add other content classes; i.e., portals, positions, and team members.  We will describe in subsequent sections the contents of the YAML front matter block for each content class.  A different process, which we will also describe in a subsequent section, is used to add content about a tool to the website (tool information is drawn from the tool's public github repository).

### Updating the public site

All edits should be made to your personal `user-staging` branch. When you start work, make sure you're on your personal staging branch:

	git checkout <your name>-staging


Instead of merging your changes directly to the `master` branch, you will create a pull request.

After pushing your branch to the repo, go to github and create a pull request, naming cbirger as the reviewer.  (Eventually reviewer responsibilities will be shared across multiple lab members.)  **When you create the pull request, please make sure you set the base repository to mellislab/mellislab.github.io and the base reference branch to master.**  Once a pull request is approved, it will be merged to master.  It is the merge to master that results in the publication of the new contento to the lab's public website.

A couple of minutes after your pull request is approved and completed, check to make sure your updates to the public site [www.mellislab.org](http://www.mellislab.org) appear the way you intended.

Don't forget to make your changes on a personal staging branch so they can be reviewed before merging into master!

## Changing look and feel

Fonts, colors, spacing, and similar stylings are separate from the template pages. Like most sites, we use Cascading Style Sheets (CSS).  Much of this is borrowed from [Gad Getz's](http://getzlab.org) and [Alan Drummond's lab website](http://drummondlab.org) but may be updated over time (based on input from others.

# Content Classes

## papers

The Mellis Lab website displays a list of all publications in the "Papers" page.  In all listings, a paper's title is an internal link to a web page devoted to that paper.  The paper's web page lists all of the paper's authors; journal name, volume, issue and page; abstract; public link to journal article fulltext and/or pdf (when available); the PubMed ID (linked to PubMed entry); and the DOI, which is linked back to the paper's permanent web address. 

To add a new paper to the site, you will create a markdown file and place it in the papers/_posts folder.  The file's name must follow the following syntax:

	yyyy-mm-dd-<any short, hyphenated string>.md
	
The date should be the publication date of the paper.  I recommend using the print publication (rather than the e-publication date), but this is open for discussion.

The yaml front matter should contain the following:

	---
	layout: paper
	title: <full title of paper>
	year: <publication year>
	journal: <journal name>
	[volume: <volume of journal publication>]
	[issue: <issue of journal publication>]
	[pages: <start page>-<end page>]
	authors: <comma-separated list of authors>
	first_authors: <comma-separated list of first authors>
	senior_authors: <comma-separated list of senior authors>
	corresponding_authors: <comma-separated list of corresponding authors>
	[explicit_nonlab_citations: <comma-separated list of nonlab author citation names that match lab members' citation names>]
	[fulltext: <url to full-text journal html>]
	[pdflink: <url to full-text journal pdf>]
	[pmid: <pubmed ID>]
	doi: <assigned doi>
	category: paper
	[preprint: false | true]
	---
	
IMPORTANT:
 
- The title and year values need to be surrounded by double quotes; other field values do not (we are working on figuring out the source of this quirk and eliminating the need for quotes).  Enclosing any of the values in quotes, however, should never do any harm.
- key/value pairs in brackets are optional; you may also list the key of an optional pair with no accompanying value

To get the comma-separated list of authors (which can be quite long for many of our lab's publications) go to the paper's pubmed site, click on "Cite" under "ACTIONS" and copy/paste the full length of authors.  Note each author is listed lastname first followed by one or two initials, e.g.:

Mellis IA, Wu M,  Hong H, Tzang CC, Bowen A, Wang Q, Gherasim C, Pierce VM, Shah JG, Purpura LJ, Yin MT, Gordon A, Guo Y, Ho DD 
(The PubMed site is also a good source for the other fields.)

Each current or past lab member's page contains a yaml key/value pair `citation-names`.  It is important that any citation name used for that member be included in that key's value.  For example, in pmid citations, Ian's name is listed as Mellis IA or Mellis I;  his team member page contains the following comma-separated list:

	citation_names: Mellis IA, Mellis I
	
This is how we ensure lab members' names appear in a bold font in any paper added to the lab website.

Situations arise where a lab member's citation name is identical to the citation name of a nonlab author of a paper.  To avoid having that paper listed under the lab member's member page, we provide the optional `explicit_nonlab_citations` yaml front matter field to explicitly call out author citation names that should not be associated with a lab member. 

	explicit_nonlab_citations: Kim S

This is done on a per-paper basis.  When adding a paper to the web site, the submitter should review the author list and make sure none of the paper's author citation names collide with citation names of lab members.

The fulltext key/value pair should contain a link to the article's html text on the journal site.  The pdflink key/value pair should contain a link to the publication's full-text pdf on the journal site.

After the YAML front matter provide the paper's abstract, e.g.,

	# Abstract
	
	<paste the paper's abstract here>
	
Once the new paper is added to the papers/_posts directory, it will be fully integrated into the site; e.g., (i) it will be listed on the web site's main page as a recent paper, (ii) it will be listed on the Selected Papers page and (iii) it will be listed on the member page(s) of the lab members who are included the paper's author list.

## team

Navigating to "Team" (on the top navigation bar) displays Getz Lab membership, both present and past.  Members, some close collaborators, and alumni are listed under the following subheadings: 

- Principal Investigator (Ian)
- Lab Members
- Co-Mentees
- Friends of Lab
- Alumni

Clicking on a team member's name takes one to the member's personal lab webpage.  Each current member of the lab is responsible for creating and/or maintaining their personal webpage.  

These pages reside in the folder team/_posts.  The file's name must follow the following syntax:

	1970-01-01-<last name>-<first name>.md
	
While a date is still required, we wanted individuals to be listed in alphabetical order within each category; hence all needed to have the same date stamp...I needed an arbitrary date so I chose the Unix epoch.  IMPORTANT: your last and first names must be in lower case in the filename.
	
For many lab members a baseline page has already been created containing a minimal amount of information (name, category position, citation_names, alum status).  You should update this page with your personal information; e.g., twitter, github userid, google scholar id, linkedin id, image, cv).

The yaml front matter in a lab member page should contain the following:

	---
	layout: member
	title: <member name (first last)>
	citation_names: <comma-separated list of (PMID-style) citation names>
	category: Principal Investigator | Member | Co-Mentee | Friend
	position: <position title>
	[email: <columbia email>]
	[mask_email: false | true]
	[twitter: <twitter handle>]
	[bluesky: <bluesky handle>]
	[github: <github userid>]
	[linkedin: <linkedin id>]
	[website: <personal website url>]
	[image: /assets/images/team/<image png filename>]
	[cv: /assets/pdfs/<cv pdf filename>]
	[scholar: <google scholar id>]
    [orcid: <orcid id>]
	[disclosures: <url to lab member's disclosures statement>]
	alum: false | true
	[parting_date: <date left lab, YYYY-MM-DD]
	[other_institution: < other institution member is affiliated with>
	other_intitution_user_page: <url to user page at affiliated institution>]
	---

Front matter fields listed in brackets above are optional. 

Your image file should have an aspect ratio of 160:196.  This ensures the rows on the team page all have the same height.

The disclosures link typically points to a shared google doc that the lab member maintains.  The google doc must be public; i.e., given the link, anyone can view the document.

If alum is set to true, you must also provide a parting_date field. This allows us to list alumni in descending order of the date they left the lab.

Following the YAML front matter you may provide any text you would like to appear on your member page.  This might include research interests and biographical information.


## positions
TBD

## talks
TBD

## events
TBD

# To-dos

See Issues on [the repo site](https://github.com/mellislab/mellislab.github.io).


# License

[MIT](http://opensource.org/licenses/MIT)
