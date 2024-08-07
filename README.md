# Wiki Leet

> [Click here for a sample repo (my LeetCode repo)](https://github.com/Zanger67/leetcode)

WikiLeet is a program that takes your local LeetCode submissions and organizes them into a navigatable wiki-like set of markdown files giving you an aesthetically pleasing README to view your journey. See the link above for my own LeetCode repo as an example.

Using GitHub actions, it will run everytime you commit a new submission to your linked repo, updating all the markdown links in the process. 


## Setup

1. In your repo on github.com, click on `Actions` and ensure that actions are enabled for this repo. Then in your repo create a folder 
2. Create a `.github/workflows/` folder/directory in your repo.
```
./project root
└── .github
    └── workflows
```
3. Create a `main.yml` file in this folder
```
./project root
└── .github
    └── workflows
        └── main.yml
```
4. Paste the following code into the file. Make sure to insert your own LeetCode username!

```yml
name: '[Updating markdown files]'

on:
    # Allows for munual runs of workflow
    workflow_dispatch:

    # Default whenever a new code file is pushed to the main branch
    push:
        branches:
            - main
        paths:
            - 'my-submissions/**'

permissions:
    contents: write

jobs:
    build:
        runs-on: ubuntu-latest
        
        steps:
            - name: Call and run markdown generator
              uses: Zanger67/WikiLeet@main
              with:
                # Insert your LeetCode username here!
                username: Zanger
```

<!-- Insert gif of it running and creating all the markdowns -->

## Additional Features

- Categories
  - The NeetCode150 and Blind75 lists have been added.
  - If you put "Daily" or "Weekly Premium" in the file's name, it will categorize them as such.

- Each question
    -   Contains a completion date, last attempted date, acceptance rate, related topics, languages used, etc.
    -   Each question has a markdown generated containing all solutions (if you have multiple) with all relevant details.
    -   If you create a `.txt` or `.md` file with a question number (e.g. `1234.py`), it will be inserted as a "summary" in the question's markdown. I personally use this to place my notes that I created while working on the problem. Click [here](https://github.com/Zanger67/leetcode/blob/main/markdowns/_3213.%20Construct%20String%20with%20Minimum%20Cost.md) for an example.

- Contests
  - In your submissions folder, if you want contests to be group, you can create a folder.
  - Any questions in that folder will be attributed to the contest of the folder's name. Click [here](https://github.com/Zanger67/leetcode/tree/main/my-submissions) for an example.

- Auto-uploading submissions
  - If you use extensions that automatically pulll your submissions, as long as the question's number is present in the file name and it's not in a folder, it will work perfectly.
  - E.g. with the [vscode-leetcode](https://github.com/LeetCode-OpenSource/vscode-leetcode/tree/master) extension, you can set the `my-submissions` as the file store location and the script will parse it all the same.

- Creation and modification dates
  - Default times are set based off of UTC commit times since that's what LeetCode goes off of.
  - These are tracked based off of your commit history by default. The script parses every file associated with a question up it's commit log finding the earliest and latest dates.
  - If ran manually, this can be done using the file creation and modification dates.
  - Warning: parsing the git commit history of each file is **slow** when done locally it seems but on GitHub actions, it's relatively fast. I'm not sure why this is, but to run locally look at the `flag` instructions for `-n` and `-g` in the [main.py](main.py) file's `main()`.


### Running Locally
The program can be ran locally once imported as a submodule via running `main.py` or `main.ipynb`.

<!-- Insert instructions for submodule import and the run instructions -->

## About files and directories

| File / Directory                                                           | Details                                                                                                                                                                                                                                                      |
| :------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`question_data/`](question_data/)                                         | Contains `.json` and `.pkl` files with question information such as AC rates, related topics, titles, etc. This is done to avoid constantly querying the Leetcode server and is automated within this repo. |
| [`user_data/`](user_data/)                                                 | A `.gitignore`d folder that will store the last modified date of each question so it doesn't unnecessarily reprocess questions that haven't changed. This only applies to local runs of the script.                                                                                                         |
| [`Lists/`](Lists/)                                                         | `.txt` files with lists such as the NeetCode150 and Blind75 (see the readme in this folder for more information).                                                                                                                                                   |
| [`.env`](.env.sample)                                                      | A config file template that you should edit with your own information, namely your LeetCode username.                                                                                                                                  |
| [`main.ipynb`](main.ipynb)                                                 | The main program that parses all your code and generates the markdowns.                                                                                                                                                                                      |
| [`main.py`](main.py)                                                       | The exported version of `main.ipynb` for easier running (type `python main.py`).                                                                                                                                                                             |
| [`parse_official_question_data.ipynb`](parse_official_question_data.ipynb) | A helper script that I use to reprocess the `.json` data (you can ignore).                                                                                                                                                                                   |
| [`LeetCode Record - Publix.xlsx`](<LeetCode Record - Publix.xlsx>)         | The spreadsheet I use to track my progress with cumulative graphs and whatnot (extra)                                                                                                                                                                        |

## EXAMPLES

_[Click here to see my own repo as an example](https://github.com/Zanger67/leetcode)_

</br>

### My Repo's Homepage (the Primary README)

![Leetcode Repository Example](misc/eg_front_page.png?raw=true)

---

### A single question markdown which was generated from 2 solution files and a context file to add details

![Individual Question's Markdown Example](misc/eg_individual_question_markdown.png?raw=true)

---

### The pre-rendered markdown

![Markdown Code Example](misc/eg_markdown_code.png?raw=true)

---

![Spreadsheet Example](misc/spreadsheet_stats.png?raw=true)

<!-- ---

</br>
</br>

![Mediums Eg](misc/image.png?raw=true)

</br> -->

<!-- ---

</br>
</br>

_View of the actual markdown before being rendered_
![Markdown Code](misc/image-2.png?raw=true) -->

<!-- ---

</br>
</br>

_View of my stats spreadsheet that I used to track my progress_
![Stats from Excel Example](misc/image-3.png?raw=true) -->
## Characteristics to be Aware of
- In each new week, you may have a case where it shows a case of `+[# of questions solved] lines` `-[# of questions solved] lines`. This is likely the due to the AC rates being updated for the week.
- Occasionally, a lot of lines may be updated on the generalized files (`README.md`, `Mediums.md`, `Arrays.md`, etc.). If the question you add increases the width of a markdown file, it'll update the whole table width to match causing this. For instance, if my `Languages` column is usually just `py, java` but then for the first time I have a 3rd language for `py, java, js`, it'll udpate the column width so it "seems" like a lot of lines.


## Future Additions Planned
- Auto detection for Daily and Weekly Premium questions (e.g. commit time +/- 1 day)