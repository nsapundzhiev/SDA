The royal programmer - WW1 version

Introduction

Precisely 100 years ago The Great War (WW1) was the main event. After a particularly fierce offensive, by the end of the year the Bulgarian army will have occupied most of Serbia, pushed the French supporting forces to the South of Macedonia, and, move into defensive positions around the so called Thesaloniki front.

The war against Serbia was dynamic and face-paced (unlike many of the other fronts in the same time). To make the task of bookkeping where every soldier is currently on the front and what's his status, tzar Ferdinand has ordered to you, the royal programmer, to write a small program that tracks in what battle unit ever soldier is.


News format

The news we'll be passing to your class will one of the following types:

unit_name = [s1, s2, ...] - unit assignment - this creates a unit with the name <unit_name> and lists all of its soliders. Each of s_i is a integer and represents the index of some soldier.
<unit1> attached to <unit2> - unit attachment - from now on, all soldiers in <unit1> become also a part of <unit2>. The soldiers from <unit1> are added to the end of <unit2>. Important: this does not mean that the soldiers stop being part of <unit1>!
<unit1> attached to <unit2> after soldier <soldier_id> - unit positional attachment - same as unit attachment but all soldiers of <unit1> are placed after the soldier with identifier <soldier_id> instead of the end of <unit2>.
soldiers <s1>..<s2> from <unit> died heroically - soldier death - removes all soldiers starting from <s1> and ending with <s2> (inclusive) from all units they are currently part of. <s1> and <s2> might be the same. To make matters easier on your side, we'll tell you the highest-level unit that those soldiers are part of. (If s1 and s2 are part of unit x which has been attached to unit y, we'll give you y)
show <unit> - unit display - displays a list of all soldiers in the given <unit>
show soldier <soldier_id> - soldier display - displays a list of all units the soldier with the given <soldier_id> is in
