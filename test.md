---


---

<h1 id="machine-vision-halcon">Machine Vision: Halcon</h1>
<p>The essentials of solving vision problems with Halcon.</p>
<h2 id="introduction">Introduction</h2>
<p>During this tutorial you will learn the essentials about machine vision using Halcon software by working on different exercises.</p>
<p>Professional industrial computer vision libraries, such as Cognex or in this case Halcon, are usually made to allow a relatively inexperienced user to write really simple image processing code. Even though this is the case it is encouraged to read through the tutorial completely. With machine vision it’s all about the details and these can make or break your application.</p>
<p><strong>Goals of this practicum</strong></p>
<ul>
<li>Getting familiar with the HDevelop GUI and IDE of Halcon.</li>
<li>Being able to create a working job using the IDE.</li>
<li>Learn how to enter and extract information using Halcon.</li>
</ul>
<p>This course consists of multiple exercises. These will increase in difficulty. In the first part you will detect bottles in crates and get familiar with the GUI of Halcon. In the second part you’ll locate use-by dates on food packaging with the OCR capabilities. In the last part you’ll perform quality control on medicine packaging. There is a bonus case for learning how to deal with calibration based on the fixed world instead of on a robot-arm.</p>
<p><strong>Preparation</strong></p>
<p>In order to perform this course you will need access to a computer running the HALCON software. This software is free to try out for 30 days with the evaluation license. Follow the steps below to get the software working properly on a computer with your operating system of choice.</p>
<p><strong>- Installing the software</strong></p>
<ol>
<li>In order to download the software you have to create a customer account. You can do this by going to this page: <a href="https://www.mvtec.com/download/registration">https://www.mvtec.com/download/registration</a></li>
<li>After registering go to: <a href="https://www.mvtec.com/download/halcon/">https://www.mvtec.com/download/halcon/</a></li>
<li>Select the recent ‘steady’ version and select the operating system you wish to install HALCON on. (18.11.2.0 as of writing this guide). A download button will appear which you can press to initiate the download.</li>
<li>After completing the download install the software on your computer.</li>
<li>During installation you’ll be given the option to insert the license file. Select the license file for the ‘Steady’ version and press the install button.</li>
<li>After completing the installation open the HDevelop software and you’ll be greeted by a welcome screen and the GUI.</li>
</ol>
<p>Now you are ready to start working on the exercises.</p>
<h2 id="exercise-1-detecting-bottles-in-crates">Exercise 1: Detecting bottles in crates</h2>
<p><img src="http://media.nu.nl/m/m1mxxpra124w_wd1280.jpg/normen-afschaffing-statiegeld-niet-gehaald.jpg" alt="Statiegeld inleverpunt"></p>
<p>If you buy a large plastic bottle of soda, a beer bottle or a crate of beer, chances are you will pay more than you expected at checkout. That’s a deposit! It is a deposit on the item itself to encourage you to return the empty packaging for recycling. When you are done with the bottles you take it back to the supermarket where there is a machine where you can put the bottles/crates into. With machine vision and other detection techniques it is possible to analyse the ingested material in great detail on colour (clear or coloured) or material (can, PET, PP, glass, etc.).</p>
<p>The technical development to automatically sort the material in great detail makes recycling and administration of the system increasingly optimised. The software ensures a very fast settlement of deposit amounts and also provides a wealth of market information.</p>
<p>In this first exercise you’ll have a first look at HDevelop and its basic functionalities. You will learn how to set up our own program to analyse pictures, use counters to display the gathered information and your for-loops to repeat processing images.</p>
<h3 id="loading-images">Loading Images</h3>
<p>Lets begin by creating a new job.</p>
<blockquote>
<p>File → New Program</p>
</blockquote>
<p>Insert an pre-installed image of a bottle crate.</p>
<blockquote>
<p>Assistants → Image Acquisition 1 → Select File(s) …</p>
</blockquote>
<p>Locate the ‘bottles” folder, select the first picture &amp; press ‘open’</p>
<p>[PLAATJE1]</p>
<p>As you can see, an image has been loaded. It is displayed in the Graphics Window.</p>
<p>You can adjust the visualisation of the image in several ways. For example, you can change the size of the window, restore the aspect ratio, move the image, and zoom in and out with the mouse wheel.<br>
Instead of going through the hassle of using the menu buttons to load images it is far easier to load images with the Program Window.</p>
<p>Copy and paste the following code in the Program Window:</p>
<pre class=" language-sh"><code class="prism  language-sh">read_image (Image, ‘bottles/bottle_crate_01’)
</code></pre>
<p>When run the same picture from the ‘bottles’ folder as before is being inserted as an Iconic Variable.</p>
<p><strong>Inspection criteria</strong><br>
To determine if there are any bottles present in picture we have to set up criteria.</p>
<p>For detecting bottles we are not interested in the location. The crates are moving over a conveyor belt which stops when crates are in a pre-determined position after a camera takes a picture of each crate. This means we will only do quality checks. In this first case you will learn how to identify the right criteria to establish a proper vision application.</p>
<p>You will learn about a few tools to do a quality inspection on the crates.</p>
<p>[PLAATJE2]</p>
<p><strong>Step 1: Detecting bottles</strong></p>
<p>We start of by detecting bottles in the picture. To achieve this start by getting familiar the Operator Window. Here you can search for operators and adjust their parameters. In the Operator Window, you can see a short description of the operator.</p>
<blockquote>
<p>→ Search for the operator ‘threshold’</p>
</blockquote>
<p>You also see an overview of the parameters. In this case, we process the image and store the result in a region. The thresholds are defined by MinGray and MaxGray. The parameters are categorised into iconic, and control parameters, and these small symbols tell you whether it’s an input, or an output parameter.</p>
<blockquote>
<p>→ Insert the following parameters: <em>Image, BackgroundRegion, 50, 130.</em></p>
</blockquote>
<p>After all the parameters have been entered press enter. The Threshold operator will be added in the program. To edit an operator, double-click on it in the Program Window.</p>
<p>Do the same procedure for the next four operators.</p>
<blockquote>
<p><em>‘opening_circle’</em> → <em>BackgroundRegion, BackgroundRegion, 3.5</em></p>
<p><em>‘threshold’</em> → <em>Image, Region, 85, 255</em></p>
<p><em>‘difference’</em> → <em>Region, BackgroundRegion, Region</em></p>
<p><em>‘connection’</em> → <em>Region, ConnectedRegions</em></p>
</blockquote>
<p>Your code should look like this when finished:</p>
<p>[PLAATJE3]</p>
<p>Let’s start by running the program.</p>
<p>Press the “Run”-button on the toolbar, or F5 on your keyboard. The green arrow, or Program Counter, tells us that the program has been executed only partially so far. Let’s reset it and take a look at the whole program. You can reset the program by clicking on the “Reset Program Execution”-button or by pressing F2 on your keyboard. Then, go through your program step by step by clicking the “Step Over”-button or by pressing F6 repeatedly. Alternatively, you can click “Run” or F5 to execute the program until it is stopped explicitly, for example by the operator “stop”. Depending on the mouse position, you can reset the program counter, or you can insert a breakpoint. Furthermore, you can deactivate and activate single lines of your program very quickly. Simply select the line and click the respective buttons on the toolbar, or use F3 and F4. Now, let’s execute the program until the end.</p>
<p>The “Variable Window” displays all variables. They are divided into Iconic Variables, which contain images, regions, and contours, and Control Variables, which contain all numbers and strings.</p>
<p>You can quickly display iconic variables in the Graphics Window by double-clicking on them in the Variable Window. Alternatively, you can right-click on them and choose “display” or “clear/display”. “Display” overlays the new object on top of the existing display in the Graphics Window, whereas “Clear/Display” clears the Graphics Window first.</p>
<p>You’re almost there, lets continue to add a few operators to reduce the noise in the picture.</p>
<blockquote>
<p><em>‘select_shape’</em> → <em>ConnectedRegions, Candidates, [‘width’,‘height’], ‘and’, [25,25], [100,100]</em></p>
<p><em>‘fill_up’</em> → <em>Candidates, FilledCandidates</em></p>
<p><em>‘opening_circle</em> → <em>FilledCandidates, FilledCandidates, 15.5</em></p>
</blockquote>
<p>After reducing noise lets extract the ‘round’ reflections from the bottles.</p>
<blockquote>
<p><em>‘select_shape’ → FilledCandidates, RoundCandidates, ‘circularity’, ‘and’, 0.87, 1</em></p>
</blockquote>
<p>All that rests us is to display the results.</p>
<p>Copy/Paste the following code to your code:</p>
<pre><code>_dev_display (Image)_
_dev_set_line_width (5)_
_dev_set_color ('lime green')_
_dev_display (RoundCandidates)_
</code></pre>
<p>Your code should look like this after these steps:</p>
<p>[PLAATJE4]</p>
<p>Run your code and you’ll be presented by 20 green circles indicating that there are 20 bottles present in this crate.</p>
<p>[PLAATJE5]</p>
<p><strong>Step 2: Extract amount of bottles</strong></p>
<p>After having detected the bottles present in the crates lets insert a counter. This will inform the system how many bottles have been inserted so it can calculated how much deposit should be returned to the customer.</p>
<p>Add this line in front of your code:</p>
<pre><code>dev_open_window_fit_image (Image, 0, 0, -1, -1, WindowHandle)
</code></pre>
<p>Add these lines at the end of your code:</p>
<pre><code>count_obj (RoundCandidates, NumBottles)
set_display_font (WindowHandle, 16, 'mono', 'true', 'false')
disp_message (WindowHandle, NumBottles + ' bottles found.', 'window', 12, 12, 'black', ‘true')
</code></pre>
<blockquote>
<p>Run your code →</p>
</blockquote>
<p>[PLAATJE6]</p>
<p><strong>Step 3: Detecting upside down bottles</strong></p>
<blockquote>
<p>Double click the “read_image” operator and change the picture to “bottle_crate-08”</p>
</blockquote>
<p>If we run the current code through other pictures, we can see that upside down bottles are detected as correctly positioned ones. This is a problem we need to solve.</p>
<p>[PLAATJE7]</p>
<p>With the select_shape operator it is possible to exclude circles that are larger than a certain diameter.</p>
<blockquote>
<p>Add the following after the other select_shape operator.</p>
</blockquote>
<pre><code>select_shape (RoundCandidates, BigBottles, 'max_diameter', 'and', 75, 99999)
</code></pre>
<p>To highlight these bigger circles we add a few lines to the ‘display results’ section.</p>
<pre><code>dev_set_color ('goldenrod')
dev_display (BigBottles)
</code></pre>
<p>If you want it is possible to select another color.</p>
<p>Running the program will result in this:</p>
<p>[PLAATJE8]</p>
<p><strong>Step 4: Detecting clutter on the crate</strong></p>
<blockquote>
<p>Double click the “read_image” operator and change the picture to “bottle_crate-12”</p>
</blockquote>
<p>If we run the code on this picture, we can see that because of the clutter laying on top of the bottles a couple of bottles are not detected. This will result in clutter being accepted by the machine and an angry customer because only receive a part of their deposit will be returned. This problem can be solved by giving off a warning to the machine that the crate contain clutter. So these crates will be rejected. In order to detect this clutter we will add more lines of code to the program.</p>
<p>[PLAATJE9]</p>
<p>To detect the clutter we can detect the large region of reflected light. With the same operator used to reduce the noise it is possible to create a clutter region.</p>
<p>Copy the following code to your program window</p>
<pre><code>select_shape (ConnectedRegions, Clutter, ['width','height'], 'or', [100,100], [500,400])
opening_circle (Clutter, Clutter, 8.5)
difference (ConnectedRegions, Clutter, Region)
connection (Region, ConnectedRegions)
</code></pre>
<p>This will produce an Iconic Variable which we named ‘Clutter’.<br>
We can now count the number of times ‘Clutter’ show up and send out a warning.</p>
<p>Add a ‘count_obj’ operator after the counter we added before. With the object being Clutter and the output is NumClutter.<br>
NumClutter will be used to display a warning.</p>
<p>Add these lines to the end of your code:</p>
<pre><code>if (NumClutter &gt; 0)
    disp_message (WindowHandle, 'Warning! Clutter detected!', 'window', 50, 12, 'red', 'true')
endif
</code></pre>
<p>Run the program again and if you did everything correctly a warning message will be displayed in the Graphic Window.</p>
<p><strong>Step 5: Loop the program to process multiple pictures</strong></p>
<p>Now we have set up all of the inspection criteria we have one problem remaining. Every time a new crate enters the system the picture of the crate has to be manually loaded into the program. This is something we want to avoid. Lets add a loop to repeat the program for all the pictures in the bottle folder.</p>
<p>Add this line after the first line of your code:</p>
<pre><code>    for Index := 1 to 24 by 1
</code></pre>
<p>Change the ‘read_’image’ operator to:</p>
<pre><code>    read_image (Image, 'bottles/bottle_crate_' + Index$'.02')
</code></pre>
<p>Add these lines to the end of your code to run all of the pictures in the folder:</p>
<pre><code>    if (Index &lt; 24)
	    disp_continue_message (WindowHandle, 'black', 'true')
    endif
    stop ()
endfor
</code></pre>
<p>After all these steps you’ll have a fully working vision program to detect and count bottles plus warning the machine when there is clutter on the crate. Try it out by running the program from the start!</p>
<h2 id="exercise-2-reading-the-use-by-date-on-packaging">Exercise 2: Reading the use-by date on packaging</h2>
<p><img src="https://www.rd.nl/image/policy:1.612600:1467452107/houdbaarheidsdatum.jpg?f=16x9&amp;$p$f=ab4ea61" alt="Houdbaarheidsdatum"></p>
<p>A use-by date can be found on foods that can only be kept for a short time, such as meat, fish, pre-cut vegetables, chilled meals and fresh fruit juices.</p>
<p>This date is the last day on which you can safely use the product. After this date, pathogens, such as bacteria, can start to grow. You often can’t see, smell or taste these, but they can make you sick. So after the use-by date you have to throw the product away.</p>
<p>During the distribution fase an error has been made and a large shipment of yoghurt has been mixed with older samples. It is crucial to sort out this batch of products to prevent people getting rotten yoghurt but will take days if done by hand. This problem can be solved fast with machine vision.</p>
<p>In this second exercise we take a look at HDevelop and its Assistant functionalities. You will learn how to generate a program to detect symbols, segment these symbols and show the results in a table.</p>
<h3 id="ocr-assistant">OCR assistant</h3>
<p>To solve this problem we can make use of the Assistant options in Halcon. With one of these assistants, you can perform optical character recognition (OCR). By specifying the region of the text, adjusting values, learning examples and classifying with an existing classifier or training your own classifier, the text can be localised and read.</p>
<p>Using the OCR Assistant is simple: either choose the Quick Setup to load an image and perform an OCR by setting basic parameters or use the sections Image Source and Region of Interest where you can also load a sample and mark the text that should be read with a rectangle. Then improve the segmentation by adapting relevant parameters, choose a pre-trained font or performing your own training and finally add the resulting code to your application.</p>
<p>To start up this assistant head over to the ‘Assistants’ panel and select ‘Open New OCR’</p>
<ol>
<li>Load a sample image<br>
Search for the ‘ocr’ folder and select the yoghurt_lid_04 image.</li>
<li>Mark the position of the text to be read using a rectangle</li>
<li>Enter the text that you expect to be read</li>
<li>Check any of the statements that apply</li>
<li>Click the ‘Apply Quick Setup’ button</li>
</ol>
<p>It is possible the program will give an error saying:<br>
”Expected number of symbols were found but not read correctly.”</p>
<p>When this happens go to the ‘Segmentation’ tab and ‘OCR Classifier’ tab to change the settings until the symbols are recognised or change the text you expect to be read.</p>
<p>With the OCR assistant it is either hit or miss. Even if you manage to get the Assistant program up and running, using the setup you made might not work for other pictures. This because the Segmentation options are limited. In order to fully get access to the segmentation options it is recommended to make use of the Operators instead of the Assistant.</p>

