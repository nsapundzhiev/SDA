The royal programmer - WW1 version

Introduction

Precisely 100 years ago The Great War (WW1) was the main event. After a particularly fierce offensive, by the end of the year the Bulgarian army will have occupied most of Serbia, pushed the French supporting forces to the South of Macedonia, and, move into defensive positions around the so called Thesaloniki front.

The war against Serbia was dynamic and face-paced (unlike many of the other fronts in the same time). To make the task of bookkeping where every soldier is currently on the front and what's his status, tzar Ferdinand has ordered to you, the royal programmer, to write a small program that tracks in what battle unit ever soldier is.

Since the program will be mainly used by people who've never seen a computer, your program should work with text. For example, this one:

brigade1 = [1, 2, 3, 4, 5]
brigade2 = [6, 7, 8, 9, 10, 11]
division1 = []
brigade1 attached to division1 # division1 == [1, 2, 3, 4, 5]
brigade2 attached to division1 # division1 == [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
soldiers 4..10 from division1 died heroically  # division1 == [1, 2, 3, 11], brigade1 == [1, 2, 3], brigade2 == [11]
brigade3 = [15, 16, 23]
brigade3 attached to division1 after soldier 3 # division1 == [1, 2, 3, 15, 16, 23, 11]
show division1

Output: [1, 2, 15, 16, 23, 3, 11]
(the comments after # shows the state of the division)

Formal definition

Your task is to implement the following Java interface:

public interface IFrontBookkeeper {
    String updateFront(String[] news);
}
in a class named exactly FrontBookkeeper<fn> where <fn> is your faculty number. For example, if your faculty number is 12345:

public class FrontBookkeeper12345 implements IFrontBookkeeper {
    public String updateFront(String[] news);    
}
The lone method updateFront will accept an array of strings, each containing a line in the format shown above.

News format

The news we'll be passing to your class will one of the following types:

<unit_name> = [s1, s2, ...] - unit assignment - this creates a unit with the name <unit_name> and lists all of its soliders. Each of s_i is a integer and represents the index of some soldier.
<unit1> attached to <unit2> - unit attachment - from now on, all soldiers in <unit1> become also a part of <unit2>. The soldiers from <unit1> are added to the end of <unit2>. Important: this does not mean that the soldiers stop being part of <unit1>!
<unit1> attached to <unit2> after soldier <soldier_id> - unit positional attachment - same as unit attachment but all soldiers of <unit1> are placed after the soldier with identifier <soldier_id> instead of the end of <unit2>.
soldiers <s1>..<s2> from <unit> died heroically - soldier death - removes all soldiers starting from <s1> and ending with <s2> (inclusive) from all units they are currently part of. <s1> and <s2> might be the same. To make matters easier on your side, we'll tell you the highest-level unit that those soldiers are part of. (If s1 and s2 are part of unit x which has been attached to unit y, we'll give you y)
show <unit> - unit display - displays a list of all soldiers in the given <unit>
show soldier <soldier_id> - soldier display - displays a list of all units the soldier with the given <soldier_id> is in
Some additional syntax constraints:

Each of the news will be given on a new line.
The total number of news will never be more than 100 000.
The total number of soldiers will never be more than 100 000.
The total number of units will never be more than 10 000.
Unit names will never contain whitespace (interval, tab, etc.)
Soldier identifiers will be unique.
When listing the soldiers in a given unit, they'll be separated by a comma and a single whitespace. For example - [1, 2, 3, 4].
Your output should contain one line for every show command. No trailing and no leading whitespace is allowed. Be strict about this constraint as your homework will be automatically checked and any difference will get you 0 points.
All test data will be correct. There will be no errors in the input string, no soldiers will die before they are created and no units will be mentioned before they are defined. You don't need to test or guard against incorrect data.
And a few semantics constraints:

Units can only be attached to at most one other unit. Attempts to attach a unit a second time, should detach it from the previous one.
Soldiers, however, can be in as many units as possible. The following example should result in squad, patrol, company, battalion:
squad = [1]
patrol = []
company = [] 
battalion = []
squad attached to patrol # patrol becomes [1]
patrol attached to company # company becomes [1]
company attached to battalion # battalion becomes [1]
show soldier 1
When a soldier dies, he should be removed from all units he's in.
When a range of soldiers die, they'll be always specified in the correct unit and order.
When units are attached with a position, they'll only be attached to the end of another unit, never in the middle of another unit. For example:
x = [1, 2]
y = [3]
z = []
x attached to z
y attached to z after soldier 1 # this will NEVER happen, because 1 is in the middle of unit x.
# however, this is fine:
y attached to z after soldier 2 # z not contains [1, 2, 3]
Furthermore, units will only be attached to topmost level e.g.:
x = [1, 2]
y = [3]
z = []
w = []
x attached to z
y attached to z
z attached to w
q = [4]
# This will NEVER happen, because soldier 2 is inside z which it's not the end of a unit at topmost level
q attached to w after soldier 2
# This is fine - q is attached at the end of z
q attached to w after soldier 3
Important: Units will either contain only soldiers or only other units. The following example will never happen:
x = [1, 2]
y = [3]
x attached to y # y cannot contain both a soldier and another unit
*Important: * Soldiers which are used as range limits, we'll probably be used more than once. In other words, if soldier <soldier_id> was mentioned in one of the news (for example unit positional attachment) it's highly likely he'll be mentioned again (in another positional attachment, show soldier or soldier death). Use this fact to optimize your code.
Here's one really big example that shows all the options in actions. The parts after # are comments and are not part of the example, they are only there to improve your understanding of the problem:

Input

regiment_Stoykov = [1, 2, 3]
show regiment_Stoykov
regiment_Chaushev = [13]
show soldier 13
division_Dimitroff = []
regiment_Stoykov attached to division_Dimitroff
regiment_Chaushev attached to division_Dimitroff
show division_Dimitroff
show soldier 13
brigade_Ignatov = []
regiment_Stoykov attached to brigade_Ignatov  # brigade_Ignatov == [1, 2, 3]
regiment_Chaushev attached to brigade_Ignatov after soldier 3 # brigade_Ignatov == [1, 2, 3, 13]
show brigade_Ignatov
show division_Dimitroff # both regiments detached from division_Dimitroff, so that's empty now
brigade_Ignatov attached to division_Dimitroff # division_Dimitroff == [1, 2, 3, 13]
show division_Dimitroff
soldiers 2..3 from division_Dimitroff died heroically 
show regiment_Stoykov
show brigade_Ignatov
show division_Dimitroff
Expected output:

[1, 2, 3]
regiment_Chaushev
[1, 2, 3, 13]
regiment_Chaushev, division_Dimitroff
[1, 2, 3, 13]
[]
[1, 2, 3, 13]
[1]
[1, 13]
[1, 13]
