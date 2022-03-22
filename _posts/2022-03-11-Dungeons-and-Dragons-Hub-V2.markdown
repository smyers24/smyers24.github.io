---
layout: post
title:  "Dungeons and Dragons Hub in C# - V2"
date:   2022-03-11 17:00:00 -0500
categories: technical
---

## INTRODUCTION
This post is still a WIP. I need to go back and give it a more thorough pass. This was just to start writing entries again.

I previously made a post about V1 of my Dungeons and Dragons app. I made this app in an earlier point in my programming career. I had a lot to learn, and still do to this day. With that said, I've made significant improvements to the app in this version. I'm sure in a couple of years I will have even more improvements to make.

## Usage and Screenshots
* The following information are subject to change and may be outdate
![Main form](https://user-images.githubusercontent.com/29739357/153893679-ba1e5a04-d280-4009-bb78-50bb21b932ad.png)
There are several boxes in the main portion of the program: Chacter, File Management, and Roll.

#### Data Persistence
V2 has a much better method of managing files - JSON. The main areas: Character, Actions, Skills, SavingThrows, and Abilities. These areas will all be serialized to a JSON file that the program can read or save. This makes it much easier to re-load your data at a future date.

Additionally, there is a 'Generate Seed Data' button that will fill some generic data into all of the boxes. This makes it easier to fill out data from the get-go. It also would enable quick generation of a generic JSON file for usage in other places.

TODO: Add sample schema to the repo

#### Character
This box contains the character 'Name', Level, HP (Hit Points / Health), Armor Class, Initiative, and Walking Speed. These values are core to many functions of playing a character

#### File Management
The file management box is used to open any files/URLs that might need to be opened for a given session. It accepts a .txt file with line-separated links. The program will automatically open all of the links in the computers default program. For example, one might want to launch wikipedia.org, google.com, and github.com. A sample text file might look like this ![File Opening](https://github.com/smyers24/smyers24.github.io/blob/master/_site/assets/blog_images/dndHub_OpenThings.png?raw=true)<br>
Click 'Open List of Things', navigate to the file, and hit okay. The links will open. Additionally, it will populate a listbox which contains the opened links. Simply double click a link in the box to re-open it.

(WIP)
The 'Open Map' button is an easy way to have the map open at all times. After clicking this button, navigate to the saved map file. Click OK, and it will open in the 'Map' tab at the top of the program. Eventually this will also support URLs, and the tab control may change to a side bar.

#### Skills
This box will feature all of the skills that a character would have, such as Athletics or Persuasion. This can be clicked to roll the value. Additionally, a bonus and proficiency can be entered. These values will be serialized.

#### Actions
This box shows the actions that your character is able to make. It features the roll string (e.g. 1d4 + 3) that corresponds to the action, as well as a description so that you can remember exactly what the effects are.

#### Abilities
Abilities are some of the core stats that a character has: Strength, Dexterity, Consitution, Intelligence, Wisdom, and Charisma. These values set many of the other stats. They also have corresponding modifiers, which will be serialized.

These rows can be clicked to roll.

#### Roll
This is the most complicated of the boxes. It features a 'Dice Box' which has boxes for all of the accepted DnD dice. (OUTDATED - SEE OVERALL VIEW FOR UPDATED)
![Dice Box](https://github.com/smyers24/smyers24.github.io/blob/master/_site/assets/blog_images/dndHub_DiceBox.png?raw=true) <br>

###### Rolling Dice
The user can enter the number of dice rolls to the left of the corresponding dice button. Additionally, they can enter a modifier to the right of it. For example, if I wanted to roll 4, 8 sided dice and add 2 to the final sum I would enter 4 in the textbox immediately to the left of the 'd8' button. Then I would enter 2 in the box to the right of it. Finally, click the d4 button. A label to the right will update with the total. This roll would be called 4d8 + 2. 

At present, the RegEx parsing doesn't support negative modifiers. So a -2 would still count as a +2. 
In the picture above, the following rolls were entered: 5d4 (result 10), 5d8 (result 24), and 1d12 (result 8).
The next button of note is the 'Roll' button immediately below the dice. This button will roll all of the dice above that have valid values entered in their respective text boxes. It will then sum up their total and display it to the right. In the image above the total roll was 42. 

ADV stands for advtange and means it will roll twice, taking the higher of the two rolls. DIS stands for disadvantage and will roll twice, taking the lower of the two rolls.


###### Manual String Rolls
Another roll option is to type in a string in the 'Roll String' textbox. An example '1d4 + 3d6 + 4' roll would be the same as if the user entered the values in with the respective buttons above. The RegEx will also let the user know if they enter an invalid dice option, such as 'd13'. The save roll check box will not clear the string after pressing 'Manual Roll'. This is the default behavior because the user is often not re-rolling the same custom string multiple times in a row.

###### Unit Tests
There are a couple of tests right now. I need to write more. 

## Notes