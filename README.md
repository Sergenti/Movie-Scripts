# IMSDb Movie Script Parser
A python parser for movie scripts.
The project has been conducted during the class "Object Oriented Programming in Python", given by Davide Picca and assisted by Coline Métrailler during the semester of spring 2020.

We used a database of 1119 english-speaking movie scripts in plain text format (.txt), retrieved from the [Internet Movie Script Database](https://www.imsdb.com/). 

The parsed JSON files we created can be found in the `parsed_scripts` folder of this repository.

# Dependencies
- Python 3
- [dataclasses_json](https://pypi.org/project/dataclasses-json/)

# Usage
Create an instance of the `ScriptParser` class. Call the `parse()` method with a string corresponding to an entire movie script. It outputs a `Movie` instance.

```python
from ScriptParser import ScriptParser

parser = ScriptParser()

parsed_movie = parser.parse(my_movie_string)
```

The full pipeline that we used to generate the JSON files using the parser can be found in the `main.py` script.

# Data
The parser returns data that is structured as follows. They can be converted into JSON format using the `to_json()` method, thanks to the `dataclasses_json` library.

## Movie
This class represents a parsed movie. It regroups metadata, and a list of all the characters speaking in the script.

| Attribute     | type              | Description  |
| ------------- |-------------------| ------------ |
| `Title`         | `str`             | the title of the movie |
| `Author`        | `list(str)`       | a list of the script's writers |
| `Genre`         | `list(str)`       | a list of the movie's genres |
| `Characters`    | `list(Character)` | a list of the characters speaking in this movie

## Character
This class represents one of the movie's characters. It is contained in a `Movie`'s `Characters` list.

A `get_replies()` method allows access to the list of replies.

| Attribute     | type              | Description  |
| ------------- |-------------------| ------------ |
| `Character`     | `str`             | the name of the character |
| `Replies`       | `list(Reply)`     | a list of the character's replies in the script |

## Reply
This class represents one reply spoken by a character. It is contained in a `Character`'s `Replies` list.

A `get_start()` and a `get_end()` method allows access to the corresponding attribute value.

| Attribute  | type  | Description  |
| ---------- |-------| ------------ |
| `Reply`      | `str` | the text of this reply |
| `Didascalie` | `str` | the text of the didascalie. If there is no didascalie, the default value is an empty string. |
| `Start`      | `int` | start position in number of characters from the start of the script |
| `End`        | `int` | end position in number of characters from the start of the script |

# Parameters
You can overwrite or change certain attributes of a `ScriptParser`, in function of your needs.
| Attribute           | type  | Description  |
| ------------------- |-------| ------------ |
| `minimum_replies`     | `int` | The minimal amount of replies under which a character is rejected. The default value is 1. This discards all characters with no replies. With the value adjusted to 2, most false-positives will be eliminated, but also some minor characters with it. |
| `character_blacklist` | `list(str)` | A list of strings. If any of the strings in the list is contained in a character name, the character will be rejected. |
| `reply_blacklist`     | `list(str)` | A list of strings used to filter replies. If any of the strings are found in the reply (case sensitive), the reply will be rejected |

`character_blacklist` and `reply_blacklist` already contain some black listed words (e.g. `INT.` or `NIGHT`). You can completely overwrite the list, or append additional words:

```python
parser = ScriptParser()

# add words to the blacklist
parser.character_blacklist += ["NEW", "WORDS"]

# overwrite the blacklist
parser.character_blacklist = ["ENTIRELY", "NEW", "WORDS"]
```

# Contributors
- Melinda Femminis
- Florian Rieder
- Andres Stadelmann