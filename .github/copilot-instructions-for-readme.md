## Do

Should appear in the first ten or fifteen lines (as hard wrapped to no more than 80-100 cols)

###  Do include a short and complete description of the functionality and purpose of the software as the first line in the readme.
        e.g. make is a command line tool for running builds based on a rules-based Makefile
###  Do hyperlink the first word (the name of your package) to its official website, even if it is the same repo page on which the README appears. This is because people might be cloning the repo and viewing the README in other contexts, or offline.
        e.g. [make](https://www.gnu.org/software/make/)
###  Do include the primary programming language name in the first sentence description.
        e.g. written in [4D](https://4d.com) (4D is a high level developement platform)

###  Do include the license name in the first sentence description, and link to the license’s info page.
        e.g. and released under the [GPL Version 3](https://opensource.org/license/gpl-3-0).

###  Do include the approximate date of original release, as well as the current version and release date.

###  If this is a corporate project mention the name of the company.
        Link the first appearance of the company name to the company’s official website.
        e.g. developed by [FooCorp](https://foocorp.example) as part of a commercial service offering

###  Do mention the goals of the project.
        e.g. to provide a simple, fast, and reliable way to manage dependencies in a large ES6 codebase
        e.g. to efficiently ingest terabytes of sensor data collected from the experimental Foo reactor at the University of FooBar

###  Do make it clear who you envision the end user of the software to be.
        e.g. for use by developers of large, complex, multi-language projects
        e.g. for use by developers as a development tool
        e.g. for use by security administrators in regulated financial environments

###  Do make extensive use of hyperlinks; err on the side of too many if you aren’t sure.
Link everything that can be linked in the first section; company names, library names, programming language names, license types, distributions, OSes, dependencies, file formats, standards documents, etc. If there isn’t an “official” site for the entity, link a Wikipedia article. Make it very easy for people to get additional context if they want it (or to ignore it if they don’t).

## Below The Fold

### Do mention whatever versioning scheme you use, whether SemVer or yyyymmdd.xx or yy.mm (like Ubuntu).

### Do include usage/invocation examples (with corresponding output) if your project is small or simple.

### Do include some text-based screenshots (<code> or <pre> blocks or triple-backticks in markdown) if applicable.
    Avoid graphical screenshots if it’s just text.

### Do include the reason your project came into existence.
    You don’t need a whole brand stor, just a sentence or two is fine.
    e.g.

### Do include some information (including code examples) of any specialty programming language features or styles that your project focuses on.
    For example, if you are a functional programming library, show some common functional programming usage examples before and after your library so people can understand why you exist.

### Do include a top level “Getting Started” section for first-time users.
    This should include a quickstart guide (possibly inline in the README, or a link to the appropriate subsection of documentation, a link to the full documentation, and a link to the community participation section.

## Participation

###  Do include an explanation of the contribution process.
    - Do you want contributions at all?
    - Do you want bug reports or issues?
    - Do you want them via email, or via GitHub issues?
    - Do you accept pull requests?
    - Can people who submit PRs reasonably expect them to be merged? As-is or with a round of review/requested changes?
    - Set expectations.

###  Do include a complete list of any contribution hard requirements.
    Link to code style guidelines or linting configuration that must pass.

## Ideology

###  Do make it clear if your project is one free software component of a larger system that includes nonfree software, or vice versa.

###  Do make it clear if your project/software primarily exists to interface with a single centralized service, and link to that service and ideally also its EULA or TOS.

###  Do make it abundantly clear if the software is going to transmit user activity data off of the device on which it is run.
    Link to the associated privacy policy, if so. (Better yet, just don’t do that, so you don’t have to warn users in advance that you publish shady software.)

## Don’t

###  Don’t assume your above-the-fold README audience has ever heard of you, your project, or your favorite new language obsession du jour.

###  Don’t assume that the reader knows industry/specialty jargon.
    It’s ok to get technical (or make your README big) but the audience is the general web-using public. It’s ok to get as opaque and obscure as you wish in the primary technical documentation, but the README generally isn’t that. It’s an introduction, with pointers to more in-depth docs.

###  Don’t use acronyms, initialisms, or abbreviations in the document without expanding them at least once at the first use.
    If it’s not onerous or obnoxious, just use the expanded versions exclusively instead of the acronyms or initialisms.

###  Don’t worry about your README getting too big as long as you have good section headings.
    If any sections get too big, you can always factor them out later to other files if necessary (and replace them with a link to that other file), but “too big” is quite big, as vertical scrolling is cheap. It’s entirely ok for your README to contain all of the primary documentation (as well as everything else listed here) for a small project.

###  Don’t obfuscate email addresses; write them correctly and in full and linkify them and make them clickable via mailto: URLs.
    It’s totally ineffective anyway; spammers and web crawlers know how to use regex too.
    The web is not the place to fight email spam, it’s in your email and network infrastructure.
