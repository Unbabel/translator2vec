# Anonymized Keystrokes Dataset

This is the repository of the anonymized keystrokes dataset, released together with the Translator2Vec paper. Two language pairs are available, En-Fr and En-De. Each sample (available in the .seqs files) consists of a *post-editing* job, where one editor corrects the output of a machine translation to improve its quality. The label of each sample is the *id* of the editor, available on the .labels files. Each sample itself is composed of a sequence of actions that the editor took to correct the translation. The time taken to do each job is also provided, in the .time files.

9 files are available per language pair:

- anonymized_train.seqs
- anonymized_train.labels
- anonymized_train.time

- anonymized_dev.seqs
- anonymized_dev.labels
- anonymized_dev.time

- anonymized_test.seqs 
- anonymized_test.labels 
- anonymized_test.time


To analyze changes in the text, a simple tokenization was applied to each state of the translation, based on whitespaces. Then, a preprocessing step was designed to move from character-level changes to token-level changes. Since it is possible to observe simultaneous changes in multiple tokens (due to mouse-selections, pasting text or noise in data), additional actions had to be considered to handle these cases, where a block of tokens is simultaneously inserted/deleted. Also, the translation is originally split in "nuggets", which typically contain one sentence each. To keep this information, actions were created to indicate a change in the nugget currently being edited.

Each action is appended with an extra info (either a number or a string), except for the *Stop* symbol, which is always followed by *None*. The actions available for each job are the following:
##### Actions that change the current state of the translation

| Action Symbol | Action       | Appended Info       |
|---------------|--------------|---------------------|
| R             | Replace      | new token           |
| I             | Insert       | new token           |
| D             | Delete       | old token           |
| BI            | Insert Block | new block of tokens |
| BD            | Delete Block | old block of tokens |

##### Actions that don't change the current state of the translation

| Action Symbol | Action            | Appended Info             |
|---------------|-------------------|---------------------------|
| W             | Wait              | seconds                   |
| J             | Jump              | number of tokens          |
| JB            | Jump Back         | number of tokens          |
| NJ            | Jump Nuggets      | number of nuggets         |
| NJB           | Jump Back Nuggets | number of nuggets         |
| MC            | Mouse Clicks      | count of mouse clicks     |
| MS            | Mouse Selections  | count of mouse selections |
| S             | Stop              | None                      |

Each action symbol is separated from its info by the first occurrence of a :

Each action is separated from other actions by a tab. Note that all occurrences of tabs inside the text have been previously converted to whitespaces.

The 50 most common strings appearing as action info (which can be either tokens or blocks of tokens) were kept. All the others were UNKed. To anonymize the dataset, each kept string was converted to <Token_XX>, and each editor id was converted to an index.


If you use this dataset, please cite the following:
```
@article{gois2019translator2vec,
  title={Translator2Vec: Understanding and Representing Human Post-Editors},
  author={G\'ois, Ant\'onio and F. T. Martins, Andr\'e},
  year={2019},
  publisher={European Association for Machine Translation}
}
```
