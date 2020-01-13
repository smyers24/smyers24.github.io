---
layout: post
title:  "Dungeons and Dragons Hub in C#"
date:   2020-01-05 17:00:00 -0500
categories: technical
---

## INTRODUCTION
Dungeons and Dragons (DnD or D&D) is traditionally a tabletop role-playing game where groups of players make characters that participate in a campaign led by a 'Dungeon Master' (DM). There is a large variety of customization options that players can choose for their character such as race, background, class, and more. This all gets put onto a character sheet, which keeps track of things such as stats and items. The DM runs the campaign and directs the flow of the story. They drive the narrative and play as the characters that are not played by other human players. One of the most commonly known aspects of DnD is the variety multi-sided dice. The d20, a 20-sided die, is the most widely known of the varieties. In total there are 6: d4, d6, d8, d10, d12, and d20. These dice are typically used to determine the outcome of a situation. Each die plays a different roll and can be used in a multitude of ways. 
As previously mentioned, this game is traditionally played in person with an actual tabletop. I play virtually with a group of friends. As such, we have a lot to manage on our computers to make sure the session goes smoothly. I developed my DnD Hub to manage all the assets that I need for a given campaign session. It can handle my rolls, file IO, character health tracking, and more. I plan to expand on this as time goes so that I dont need to open anything whenever we play.

## Usage and Screenshots
* The following information are subject to change and may be outdate
![Main form](https://github.com/smyers24/smyers24.github.io/blob/master/_site/assets/blog_images/dndHub_MainProgram.png)
There are several boxes in the main portion of the program: Chacter, File Management, and Roll.
#### Character
This box contains the character 'Name', HP (health points), and AC (armor class). The values entered in these fields will save on program close, and reload once it's launched.

#### File Management
The file management box is used to open any files/URLs that might need to be opened for a given session. It accepts a .txt file with line-separated links. The program will automatically open all of the links in the computers default program. For example, one might want to launch wikipedia.org, google.com, and github.com. A sample text file might look like this ![File Opening](https://github.com/smyers24/smyers24.github.io/blob/master/_site/assets/blog_images/dndHub_OpenThings.png)<br>
Click 'Open List of Things', navigate to the file, and hit okay. The links will open. Additionally, it will populate a listbox which contains the opened links. Simply double click a link in the box to re-open it.

The 'Open Map' button is an easy way to have the map open at all times. After clicking this button, navigate to the saved map file. Click OK, and it will open in the 'Map' tab at the top of the program. Eventually this will also support URLs, and the tab control may change to a side bar.
#### Roll
This is the most complicated of the boxes. It features a 'Dice Box' which has boxes for all of the accepted DnD dice. The GUI is still in early development and will be changing once all core features are developled. 
![Dice Box](https://github.com/smyers24/smyers24.github.io/blob/master/_site/assets/blog_images/dndHub_DiceBox.png) <br>
###### Rolling Dice
The user can enter the number of dice rolls to the left of the corresponding dice button. Additionally, they can enter a modifier to the right of it. For example, if I wanted to roll 4, 8 sided dice and add 2 to the final sum I would enter 4 in the textbox immediately to the left of the 'd8' button. Then I would enter 2 in the box to the right of it. Finally, click the d4 button. A label to the right will update with the total. This roll would be called 4d8 + 2. 
** At present, the RegEx parsing doesn't support negative modifiers. So a -2 would still count as a +2. 
In the picture above, the following rolls were entered: 5d4 (result 10), 5d8 (result 24), and 1d12 (result 8).
The next button of note is the 'Roll' button immediately below the dice. This button will roll all of the dice above that have valid values entered in their respective text boxes. It will then sum up their total and display it to the right. In the image above the total roll was 42. 
###### Saving Custom Rolls
The box with columns titled Name, Roll, Level, and Description have yet to be fully implemented. Once fully integrated, the user will be able to hit the 'Add' button. This will invoke a modal pop-up box which has fields for the previously mentioned columns. The user can enter the corresponding values and hit OK. These values will then populate the box in the respective column. To use the custom roll, the user will be able to double click it and the total will appear below. Additionally, single-clicking the roll will allow for modifying or deleting the entry.
###### Manual String Rolls
Another roll option is to type in a string in the 'Roll String' textbox. An example '1d4 + 3d6 + 4' roll would be the same as if the user entered the values in with the respective buttons above. The RegEx will also let the user know if they enter an invalid dice option, such as 'd13'. The save roll check box will not clear the string after pressing 'Manual Roll'. This is the default behavior because the user is often not re-rolling the same custom string multiple times in a row. 

## Notes
The Advantage/Disadvantage boxes are not currently implemented but will be in the future. Advantage means the user can roll twice and take the higher of the two rolls. Disadvantage is the opposite, with the user taking the lower of the two. 