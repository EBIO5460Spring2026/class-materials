### Homework to get started

See also reading for next week. This homework is not assessed. Aim to finish everything before class on Tuesday. If you're having Git troubles, feel free to email me.



#### 1. Email me your Github username

If you don't already have a GitHub account, sign up for one. Email me your GitHub username so I can add you to the class organization. This is where you will submit assignments. If you were in my class last semester, no need to send this as I have it already, unless you want to change the account you use.



#### 2. Git & GitHub refresher or intro

If you are new to Git and GitHub, or you need a git refresher, you can use the tutorials from fall 2025 [here](https://github.com/EBIO5460Fall2025/class-materials/tree/main/skills_tutorials). Relevant sections are:

* [git00_resources.md](https://github.com/EBIO5460Fall2025/class-materials/blob/main/skills_tutorials/git00_resources.md)
* [git01_setup.md](https://github.com/EBIO5460Fall2025/class-materials/blob/main/skills_tutorials/git01_setup.md) (tailored to R; for Python users see git00_resources)
* [git03_basics.md](https://github.com/EBIO5460Fall2025/class-materials/blob/main/skills_tutorials/git03_basics.md)
* [git04_amend.md](https://github.com/EBIO5460Fall2025/class-materials/blob/main/skills_tutorials/git04_amend.md)
* [git06_gitgui.md](https://github.com/EBIO5460Fall2025/class-materials/blob/main/skills_tutorials/git06_gitgui.md)




#### 3. Take control of your GitHub repo for this class

After you send me your GitHub username, I'll set up a GitHub repo for you that is within the private GitHub space for this class. I'll email you when it's ready to go. This repo is not public (i.e. not open to the world). You and I both have write access to this repo. Clone it to your computer using whatever method you prefer.   I usually do this from the command line as follows:

1. Go to the class GitHub organization: https://github.com/EBIO5460Spring2026.
2. Find your repo. It should be visible on the `Repositories` tab. Your repo is called `ml4e_firstname-lastinitial`.
3. From the green `Code` button in your repo on GitHub, copy the repo's URL to the clipboard.
4. Open a terminal on your computer and use `cd` to navigate to the location you want to put your repository.
5. Type `git clone` and paste the URL you just copied. It will look something like this:
   ```bash
   git clone https://github.com/EBIO5460Spring2026/ml4e-brett-m.git
   ```

In the repository you just cloned, you'll find a file called `README.md`. This file was created by GitHub. This is a plain text file with extension `.md`, indicating that it is a file in Markdown format. You can edit this file using a text editor. Otherwise, your repository is empty and ready for you to add files to it.



#### 4. (optional) Install the Positron IDE

   * IDE: integrated development environment
   * https://positron.posit.co/
   * Positron is new but is undoubtedly the future. It has great integration for multiple programming languages and is optimized for data science. It's based on VSCode but brings the best features of RStudio to both Python and R.
   * Windows: during installation select `add "open with positron action" to context menus`
