# 4-x-eight-segment-display-for-arduino
Arduino code for interfacing 4x eight segment display

This is my first atempt to create a 8 segment display driver.<br>
So please do not judge me very hard for my code mistakes :).


<h2>Bill of materials</h2>
4x 8 segment displays (i have HDSP-7502)<br>
8x 330 ohm resistors<br>
4x PNP transistors<br>
1x Arduino (I used a nano V3)<br>
<br>

<h2>Connections</h2>
Each segment of the display is marked with a letter. See the below image.
This type of display (HDSP-7502) has a common anode (+) and each segment is controlled with (-).<br>
Every display unit has it`s (PNP) transistor. The anode pin of the display is connected to the <strong>collector</strong> pin of the transistor.
The <strong>emitter</strong> is connected to +5v and the <strong>base</strong>.<br>
<br>
The segments are connected in parallel. Segment <strong>a</strong> from display unit 1 is connected to segment <strong>a</strong> of display unit 2, 3 and 4.<br>
Each termination of the segments is connected in series with a 330 ohm resistor to the arduino pins:<br>
<span>
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/7_segment_display_labeled.svg/220px-7_segment_display_labeled.svg.png"><br>Source: Wikipedia</span><br>


<span><strong>Table of Connections</strong></span>
<table>
  <tr>
    <th>Segment/Display</th>
    <th>Connected to</th>
    <th>Arduino pin</th>
  </tr>
  <tr>
    <td>a</td>
    <td>330 ohm</td>
    <td>6</td>
  </tr>
  <tr>
    <td>b</td>
    <td>330 ohm</td>
    <td>7</td>
  </tr>
  <tr>
    <td>c</td>
    <td>330 ohm</td>
    <td>8</td>
  </tr>
  <tr>
    <td>d</td>
    <td>330 ohm</td>
    <td>9</td>
  </tr>
  <tr>
    <td>e</td>
    <td>330 ohm</td>
    <td>10</td>
  </tr>
  <tr>
    <td>f</td>
    <td>330 ohm</td>
    <td>11</td>
  </tr>
  <tr>
    <td>g</td>
    <td>330 ohm</td>
    <td>12</td>
  </tr>
  <tr>
    <td>dp</td>
    <td>330 ohm</td>
    <td>13</td>
  </tr>
  <tr>
    <td>Display 1</td>
    <td>Transistor</td>
    <td>A2</td>
  </tr>
  <tr>
    <td>Display 2</td>
    <td>Transistor</td>
    <td>A3</td>
  </tr>
  <tr>
    <td>Display 3</td>
    <td>Transistor</td>
    <td>A4</td>
  </tr>
  <tr>
    <td>Display 4</td>
    <td>Transistor</td>
    <td>A5</td>
  </tr>
</table>
<br>


<h2>Code</h2>

<strong>void setup()</strong><br>
In the setup code I defined the pins for each display unit and for each segment and a short test for counting from 0 to 9 on all display units.
<br><br>

<strong>void loop()</strong><br>
In the loop function I created a String variable that will contain the information to be written on the Display units.
In this example I just want to see the time in seconds on the displays.
After the time is parsed to the string, then the function segment_display("string") is called.
<br><br>

<strong>int segment_display(String val)()</strong><br>
This function can receive a string. This string is then put into a char array for easy manipulation.<br>
The char array is then analysed to see how many digits it contains.Example:<br>
Case 1: string="12.55" > 4 digits and 1 char //This can be used for writing the temperature from a sensor<br>
Case 2: string="1023" > 4 digits and 0 char  //This can be used for writing the analog value from A0,A1<br>
Case 3: string="7" > 1 digit and 0 char // This can be used to write numbers to the display<br>
<br>After analyseing the char array, I created a matrix with the digits to be transfered to the display unit. If the string contains a char, that char will be interpreted as a dot and then associated with the digit in front of it. Example:<br>
<strong>String="19.55"</strong>
<table>
  <tr>
    <th>Display unit</th>
    <th>Digit</th>
    <th>Dot</th>
    <th>Display will show:</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>1</td>
    <td>1</td>
    <td>0</td>
    <td>"1"</td>
    <td>Display 1 will show digit "1" without a dot (because the dot is 0)</td>
  </tr>
  <tr>
    <td>2</td>
    <td>9</td>
    <td>1</td>
    <td>"2."</td>
    <td>Display 2 will show digit "9." with a dot (because the dot is 1)</td>
  </tr>
  <tr>
    <td>3</td>
    <td>5</td>
    <td>0</td>
    <td>"5"</td>
    <td>Display 3 will show digit "5" without a dot (because the dot is 0)</td>
  </tr>
  <tr>
    <td>4</td>
    <td>5</td>
    <td>0</td>
    <td>"5"</td>
    <td>Display 4 will show digit "5" without a dot (because the dot is 0)</td>
  </tr>
</table>
After this matrix, I created a for loop that iterates the number of digits received and write the data to the display units.
Here a short delay of 3 ms is necessary because the digits must stay on for that time so that the human eye to see it.

<br><br>
<strong>int digit(int nr, int pct)()</strong><br>
In this function I defined the numbers from 0 to 9.<br>
For example, to create the "0" digit, 6 segments on the display must be turned on:
Segment a,b,c,d,e and f.<br>
To create digit "1", two segments must be on: Segment b and c.<br>


<h2>Feel free to write anything you want on the displays!</strong>
