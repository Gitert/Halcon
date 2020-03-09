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
<p>This course consists of multiple exercises. These will increase in difficulty. In the first part you will detect bottles in crates and get familiar with the GUI of Halcon. In the second part you’ll locate use-by dates on food packaging with the OCR capabilities. In the last part you’ll perform quality control on medicine packaging.</p>
<p><strong>Preparation</strong></p>
<p>In order to perform this course you will need access to a computer running the HALCON software. This software is free to try out for 30 days with an evaluation license. This license will be given to you by the tutors. Follow the steps below to get the software working properly on a computer with your operating system of choice.</p>
<p><strong>- Installing the software</strong></p>
<ol>
<li>In order to download the software you’ll have to create a customer account. You can do this by going to this page: <a href="https://www.mvtec.com/download/registration">https://www.mvtec.com/download/registration</a></li>
<li>After registering go to: <a href="https://www.mvtec.com/download/halcon/">https://www.mvtec.com/download/halcon/</a></li>
<li>Select the recent ‘steady’ version and select the operating system you wish to install HALCON on. (18.11.2.0 as of writing this guide). A download button will appear which you can press to initiate the download.</li>
<li>After completing the download install the software on your computer.</li>
<li>During installation you’ll be given the option to insert the license file. Select the license file for the ‘Steady’ version and press the install button.</li>
<li>After completing the installation open the HDevelop software and you’ll be greeted by a welcome screen and the GUI.</li>
</ol>
<p>Now you are ready to start working on the exercises.</p>
<h2 id="exercise-1-detecting-bottles-in-crates">Exercise 1: Detecting bottles in crates</h2>
<p><img src="http://media.nu.nl/m/m1mxxpra124w_wd1280.jpg/normen-afschaffing-statiegeld-niet-gehaald.jpg" alt="Deposit returnpoint"></p>
<p>If you buy a large plastic bottle of soda, a beer bottle or a crate of beer, chances are you will pay more than you expected at checkout. That’s a deposit! When you are done with the bottles you take it back to the supermarket where there is a machine where you can put the bottles/crates into. With machine vision and other detection techniques it is possible to analyse the ingested material in great detail on colour (clear or coloured) or material (can, PET, PP, glass, etc.).</p>
<p>The technical development to automatically sort the material in great detail makes recycling and administration of the system increasingly optimised. The software ensures a very fast settlement of deposit amounts and also provides a wealth of market information.</p>
<p>In this first exercise you’ll have a first look at HDevelop and its basic functionalities. You will learn how to set up our own program to analyse pictures, use counters to display the gathered information and for-loops to repeat processing images.</p>
<h3 id="loading-images">Loading Images</h3>
<p>Lets begin by creating a new job.</p>
<p>→ <em>File</em> → <em>New Program</em></p>
<p>Insert an pre-installed image of a bottle crate.</p>
<p>→ <em>Assistants</em> → <em>Image Acquisition 1</em> → <em>Select File(s) …</em></p>
<p>Locate the ‘bottles” folder, select the first picture &amp; press ‘open’</p>
<p>[PLAATJE1]</p>
<p>As you can see, an image has been loaded. It is displayed in the Graphics Window.</p>
<p>You can adjust the visualisation of the image in several ways. For example, you can change the size of the window, restore the aspect ratio, move the image, and zoom in and out with the mouse wheel.<br>
Instead of going through the hassle of using the menu buttons to load images it is far easier to load images with the Program Window.</p>
<p>→ <em>Copy and paste the following code in the Program Window:</em></p>
<pre class=" language-sh"><code class="prism  language-sh">read_image (Image, ‘bottles/bottle_crate_01’)
</code></pre>
<p>When you run the code, the same picture from the ‘bottles’ folder as before is being inserted as an Iconic Variable.</p>
<p><strong>Inspection criteria</strong><br>
To determine if there are any bottles present in picture we have to set up criteria. In the picture below are the options that the program must be able to detect.</p>
<p>[PLAATJE2]</p>
<p><strong>Step 1: Detecting bottles</strong></p>
<p>We start of by detecting bottles in the picture. To achieve this start by getting familiar with the Operator Window. Here you can search for operators and adjust their parameters.</p>
<p>→ <em>Search for the operator ‘threshold’</em></p>
<p>You will see an overview of the parameters. In this case, we process the image and store the result in a region. The thresholds are defined by ‘MinGray’ and ‘MaxGray’. The parameters are categorised into iconic and control parameters, the small symbols tell you whether it’s an input, or an output parameter.</p>
<p>→ <em>Insert the following parameters: Image, BackgroundRegion, 50, 130</em></p>
<p>After all the parameters have been entered, press enter. The Threshold operator will be added to the program. To edit an operator, double-click on it in the Program Window.</p>
<p>→ <em>Do the same procedure for the next four operators:</em></p>
<blockquote>
<p>‘opening_circle’ → _BackgroundRegion, BackgroundRegion, 3.5</p>
<p>‘threshold’ → Image, Region, 85, 255</p>
<p>‘difference’ → Region, BackgroundRegion, Region</p>
<p>‘connection’ → Region, ConnectedRegions</p>
</blockquote>
<p>Your code should look like this when finished:</p>
<p>[PLAATJE3]</p>
<p>Let’s start by running the program.</p>
<p>Press the “Run”-button on the toolbar, or F5 on your keyboard. You can deactivate and activate single lines of your program very quickly. Simply select the line and click the respective buttons on the toolbar, or use F3 and F4. For now, let’s execute the program until the end.</p>
<p>The “Variable Window” displays all variables. They are divided into Iconic Variables, which contain images, regions, and contours, and Control Variables, which contain all numbers and strings.</p>
<p>You’re almost there, lets continue to add a few operators to reduce the noise in the picture.</p>
<blockquote>
<p>‘select_shape’ → ConnectedRegions, Candidates, [‘width’,‘height’], ‘and’, [25,25], [100,100]</p>
<p>‘fill_up’ → Candidates, FilledCandidates</p>
<p>‘opening_circle → FilledCandidates, FilledCandidates, 15.5</p>
</blockquote>
<p>After reducing noise lets extract the ‘round’ reflections from the bottles.</p>
<blockquote>
<p>‘select_shape’ → FilledCandidates, RoundCandidates, ‘circularity’, ‘and’, 0.87, 1</p>
</blockquote>
<p>All that rests us is to display the results. To achieve this have a look at the four line of code below.</p>
<p>→ <em>Copy/Paste the following lines to your code:</em></p>
<pre><code>dev_display (Image)
dev_set_line_width (5
dev_set_color ('lime green')
dev_display (RoundCandidates)
</code></pre>
<p>Your code should look like this after these steps:</p>
<p>[PLAATJE4]</p>
<p>Run your code and you’ll be presented by 20 green circles indicating that there are 20 bottles present in this crate.</p>
<p>[PLAATJE5]</p>
<p><strong>Step 2: Extract amount of bottles</strong></p>
<p>After having detected the bottles present in the crates lets insert a counter. This will inform the system how many bottles have been inserted so it can calculate how much deposit should be returned to the customer.</p>
<p>→ <em>Add this line in front of your code:</em></p>
<pre><code>dev_open_window_fit_image (Image, 0, 0, -1, -1, WindowHandle)
</code></pre>
<p>→ <em>Add these lines at the end of your code:</em></p>
<pre><code>count_obj (RoundCandidates, NumBottles)
set_display_font (WindowHandle, 16, 'mono', 'true', 'false')
disp_message (WindowHandle, NumBottles + ' bottles found.', 'window', 12, 12, 'black', ‘true')
</code></pre>
<p>→ <em>Run your code to have the amount of bottles being displayed in the grapics window.</em></p>
<p>[PLAATJE6]</p>
<p><strong>Step 3: Detecting upside down bottles</strong></p>
<p>→ <em>Double click the “read_image” operator and change the picture to “bottle_crate-08”</em></p>
<p>If we run the current code through other pictures, we can see that upside down bottles are detected as correctly positioned ones. This is a problem we need to solve.</p>
<p>[PLAATJE7]</p>
<p>With the select_shape operator it is possible to exclude circles that are larger than a certain diameter.</p>
<p>→ <em>Add the following after the other select_shape operator:</em></p>
<pre><code>select_shape (RoundCandidates, BigBottles, 'max_diameter', 'and', 75, 99999)
</code></pre>
<p>→ <em>To highlight these bigger circles we add a few lines to the ‘display results’ section.</em></p>
<pre><code>dev_set_color ('goldenrod')
dev_display (BigBottles)
</code></pre>
<p>If you want it is possible to select another color.</p>
<p>Running the program will result in this:</p>
<p>[PLAATJE8]</p>
<p><strong>Step 4: Detecting clutter on the crate</strong></p>
<p>→ <em>Double click the “read_image” operator and change the picture to “bottle_crate-12”</em></p>
<p>If we run the code, we can see that because of the clutter laying on top of the bottles a couple of bottles are not detected. This will result in clutter being accepted by the machine and an angry customer because only a part of their deposit will be returned. This problem can be solved by giving off a warning to the machine that the crate contains clutter. So these crates can be rejected. In order to detect this clutter we will add more lines of code to the program.</p>
<p>[PLAATJE9]</p>
<p>To detect the clutter we can detect the large region of reflected light. With the same operator used to reduce the noise it is possible to create a clutter region.</p>
<p>→ <em>Copy the following code to your program window:</em></p>
<pre><code>select_shape (ConnectedRegions, Clutter, ['width','height'], 'or', [100,100], [500,400])
opening_circle (Clutter, Clutter, 8.5)
difference (ConnectedRegions, Clutter, Region)
connection (Region, ConnectedRegions)
</code></pre>
<p>This will produce an Iconic Variable which we named ‘Clutter’.<br>
We can now count the number of times ‘Clutter’ shows up and send out a warning.</p>
<p>→ <em>Add a ‘count_obj’ operator after the counter we added before.</em></p>
<p>With the object being Clutter and the output is NumClutter.<br>
NumClutter will be used to display a warning.</p>
<p>→ <em>Add these lines to the end of your code:</em></p>
<pre><code>if (NumClutter &gt; 0)
    disp_message (WindowHandle, 'Warning! Clutter detected!', 'window', 50, 12, 'red', 'true')
endif
</code></pre>
<p>Run the program again and if you did everything correctly a warning message will be displayed in the Graphic Window.</p>
<p><strong>Step 5: Loop the program to process multiple pictures</strong></p>
<p>Now we have set up all of the inspection criteria we have one problem remaining. Every time a new crate enters the system the picture of the crate has to be manually loaded into the program. This is something we want to avoid. Lets add a loop to repeat the program for all the pictures in the bottle folder.</p>
<p>→ <em>Add this line after the first line of your code:</em></p>
<pre><code>    for Index := 1 to 24 by 1
</code></pre>
<p>→ <em>Change the ‘read_image’ operator to:</em></p>
<pre><code>    read_image (Image, 'bottles/bottle_crate_' + Index$'.02')
</code></pre>
<p>→ <em>Add these lines to the end of your code to run all of the pictures in the folder:</em></p>
<pre><code>    if (Index &lt; 24)
	    disp_continue_message (WindowHandle, 'black', 'true')
    endif
    stop ()
endfor
</code></pre>
<p>After all these steps you’ll have a fully working vision program to detect and count bottles plus give of a warning the machine when clutter is present on the crate.<br>
Try it out by running the program from the first line of code!</p>
<h2 id="exercise-2-reading-the-use-by-date-on-packaging">Exercise 2: Reading the use-by date on packaging</h2>
<p><img src="https://www.rd.nl/image/policy:1.612600:1467452107/houdbaarheidsdatum.jpg?f=16x9&amp;$p$f=ab4ea61" alt="Use-by date on milkcartons"></p>
<p>A use-by date can be found on food-packaging that can only be kept fresh for a short time, such as meat, fish, pre-cut vegetables, chilled meals and fresh fruit juices.</p>
<p>During the distribution fase an error has been made and a large shipment of yoghurt has been mixed with older samples. It is crucial to sort out this batch of products to prevent people getting rotten yoghurt but sorting this will take days if done by hand. This problem can be solved fast with a machine vision setup.</p>
<p>In this second exercise we take a look at HDevelop and its Assistant functionalities. You will learn how to generate a program to detect symbols and segment these symbols.</p>
<h3 id="ocr-assistant">OCR assistant</h3>
<p>To solve this problem we can make use of the Assistant options in Halcon. With one of these assistants, you can perform optical character recognition (OCR). By specifying the region of the text, adjusting values, learning examples and classifying with an existing classifier or training your own classifier, the text can be localised and read.</p>
<p>Using the OCR Assistant is simple: either choose the Quick Setup to load an image and perform an OCR by setting basic parameters or use the sections Image Source and Region of Interest where you can also load a sample and mark the text that should be read with a rectangle. Then improve the segmentation by adapting relevant parameters, choose a pre-trained font or performing your own training and finally add the resulting code to your application.</p>
<p>To start up this assistant head over to the ‘Assistants’ panel and select ‘Open New OCR’</p>
<ol>
<li>Load a sample image<br>
Search for the ‘ocr’ folder and select the ‘yoghurt_lid_04’ image.</li>
<li>Mark the position of the text to be read using the rectangle drawing tool.</li>
<li>Enter the text that you expect to be read.</li>
<li>Check any of the statements that apply.</li>
<li>Click the ‘Apply Quick Setup’ button.</li>
</ol>
<p>It is possible the program will give an error saying:<br>
”Expected number of symbols were found but not read correctly.”</p>
<p>When this happens go to the ‘Segmentation’ tab and ‘OCR Classifier’ tab to change the settings until the symbols are recognised or change the text you expect to be read.</p>
<p>When you are satisfied with the results you can export the program to code and use run it outside of the assistant.</p>
<h3 id="other-assistants">Other assistants</h3>
<p>HDevelop includes assistants for other specific machine vision tasks. Each assistant provides a user interface tailored to the requirements of his task. As you have expierenced for yourself by using this interface, you can interactively set up and configure the assistant to solve a specific machine vision problem.</p>
<p>The other assistants besides OCR are:</p>
<ul>
<li>Image Acquisition - Using this assistant, you can generate code to obtain images from different sources (files, directories, image acquisition interfaces).</li>
<li>Calibration - Using this assistant, you can calibrate your camera in order to obtain information about parameters of the camera system and distortions in the image.</li>
<li>Matching - Using this assistant, you can generate code to perform form-based, correlation-based, descriptor-based and deformable matching in your HDevelop program.</li>
<li>Measurement - With this assistant you can perform a 1D measurement.</li>
</ul>
<h2 id="exercise-3-quality-control-of-medicine-packaging">Exercise 3: Quality control of medicine packaging</h2>
<p><img src="https://www.infinityqs.com/InfinityQS/media/assets/images/tabbed-content/inset-images/your-business-case/packaging/packaging-line.jpg" alt="QC of medicine"></p>
<p>For the production of high-quality medicines and their packaging, regular quality control is essential, so that the parameters meet the specifications unabated. If the quality is checked too late in the process, an entire batch can be rejected. A control-point like this can be set up using machine vision to check the content of the packaging.</p>
<p>The task of this exercise is to check the content of machine filled blisters. An image of the packaging is used to locate the chambers within a blister pack and serve as a overlay for the ROI. This overlay is then used to check the other images. Using blob analysis, the contents of each chamber are segmented and eventually classified according to shape properties.</p>
<p><strong>Step 1: Loading reference image</strong></p>
<p>Lets begin by creating a new job.</p>
<p>→ <em>File</em> → <em>New Program</em></p>
<p>Use the read_image Operator to load the ‘blister_reference’ picture from the ‘blister’ folder. Give the image an name to load it as an Iconic Variable.</p>
<p>→ <em>To create an graphics window use this line:</em></p>
<pre><code>dev_open_window_fit_image (Reference, 0, 0, -1, -1, WindowHandle)
</code></pre>
<p><strong>Step 2: Inspection criteria</strong></p>
<p>To properly conduct a quality control of the pills and to verify if the packaging should be accepted of rejected we have to set up criteria. Unlike the previous bottle exercise where location was not crucial, in this case it is of most importance. Each blister should contain 15 pills so there will be an equal amount of individual regions to verify.</p>
<p>[PLAATJE1]</p>
<p><strong>Step 3: Creating a pattern</strong></p>
<p>In the first step we make a reference pattern to easily ‘cut out’ the chambers in the other blister images using the reference picture we have already loaded. If you have not yet done this, go back to the previous step.</p>
<p>→ <em>Copy/Paste the following copy to the program window:</em></p>
<pre><code>threshold (Reference, Region, 90, 255)
shape_trans (Region, Blister, 'convex')
orientation_region (Blister, Phi)
area_center (Blister, Area1, Row, Column)
vector_angle_to_rigid (Row, Column, Phi, Row, Column, 0, HomMat2D)
affine_trans_image (Reference, Image2, HomMat2D, 'constant', 'false')
gen_empty_obj (Chambers)
</code></pre>
<p>This will align the picture and generate an empty object where the chamber will be loaded to.<br>
Next step is to generate the chambers.</p>
<p>→ <em>We can do this with the following code:</em></p>
<pre><code>dev_set_draw ('margin')_
dev_set_line_width (5)_

for I := 0 to 4 by 1
    Row := 88 + I * 70
    for J := 0 to 2 by 1
	    Column := 163 + J * 150
	    gen_rectangle2 (Rectangle, Row, Column, 0, 64, 30)
	    concat_obj (Chambers, Rectangle, Chambers)
    endfor
endfor
</code></pre>
<p>The first two lines set the drawing style and the line width.<br>
The for-loop keeps going until all 15 rectangles have been generated at the predefined positions.</p>
<p>After the rectangles have been generated the chambers are joined together and the preparations are made for the alignment of the following pictures.</p>
<p>→ <em>Add this code to your program:</em></p>
<pre><code>affine_trans_region (Blister, Blister, HomMat2D, 'nearest_neighbor')
difference (Blister, Chambers, Pattern)
union1 (Chambers, ChambersUnion)
orientation_region (Blister, PhiRef)
PhiRef := rad(180) + PhiRef
area_center (Blister, Area2, RowRef, ColumnRef)
</code></pre>
<p>[PLAATJE2]</p>
<p><strong>Step 4: Align picture to pattern</strong></p>
<p>The blisters wont be positioned in the same region, so we have to come up with a solution to solve this problem.</p>
<p>The loaded image will be aligned to this pattern and reduced to the area of interest, i.e. the chambers of the blister pack.</p>
<p>First off we load the next image from the blister folder. With the threshold and connection operators we create a connected region variable.</p>
<p>→ <em>Copy and paste the following code to your program.</em></p>
<pre><code>read_image (Image, 'blister/blister_01')
threshold (Image, Region, 90, 255)
connection (Region, ConnectedRegions)_
select_shape (ConnectedRegions, SelectedRegions, 'area', 'and', 5000, 9999999)
shape_trans (SelectedRegions, RegionTrans, ‘convex')
</code></pre>
<p>Next step is to transform the picture to align it with our reference picture.</p>
<p>This will be done by calculating the rotation with the variable Phi. Then the picture will be transformed to match the rotation of the reference picture.</p>
<p>→ <em>Add this code:</em></p>
<pre><code>orientation_region (RegionTrans, Phi)
area_center (RegionTrans, Area3, Row, Column)
vector_angle_to_rigid (Row, Column, Phi, RowRef, ColumnRef, PhiRef, HomMat2D)
affine_trans_image (Image, ImageAffineTrans, HomMat2D, 'constant', 'false')
</code></pre>
<p>Try and run the program up until the last line with the F6-key and see what the program is doing. If everything went well the last line should present you with a correctly orientated picture.</p>
<p><strong>Step 5: Segment pills</strong></p>
<p>Now we are able to create a reference frame, align pictures to this reference, create a Region Of Interest. The next step is to segment the pills from the picture.</p>
<p>→ <em>Copy and paste the following code to your program.</em></p>
<pre><code>reduce_domain (ImageAffineTrans, ChambersUnion, ImageReduced)
decompose3 (ImageReduced, ImageR, ImageG, ImageB)
var_threshold (ImageB, Region, 7, 7, 0.2, 2, 'dark')
connection (Region, ConnectedRegions0)
closing_rectangle1 (ConnectedRegions0, ConnectedRegions, 3, 3)
fill_up (ConnectedRegions, RegionFillUp)
select_shape (RegionFillUp, SelectedRegions, 'area', 'and', 1000, 99999)
opening_circle (SelectedRegions, RegionOpening, 4.5)
connection (RegionOpening, ConnectedRegions)
select_shape (ConnectedRegions, SelectedRegions, 'area', 'and', 1000, 99999)
shape_trans (SelectedRegions, Pills, ‘convex')
</code></pre>
<p>This code will import the chambers we generated earlier plus the aligned picture. It will export the pills in the blister.</p>
<p><strong>Step 6: Wrong pill types and classify segmentation</strong></p>
<p>Now will be a good time to determine when a pill should be accepted, rejected or find out it is missing.</p>
<p>How can we determine to solve these problems?</p>
<p>Since we have segmented the pills in the packaging we can calculate the area of these circles. This calculated area can be compared to predefined values to determine if a pill is correctly filled. If there is no area present we can conclude that there is a pill missing in that spot.</p>
<p>Using the if/else statement we can formulate code to solve this problem.</p>
<p>First we count the number of chambers. Then we create objects to be filled with wrong of missing pill objects.</p>
<pre><code>count_obj (Chambers, Number)
gen_empty_obj (WrongPill)
gen_empty_obj (MissingPill)
</code></pre>
<p>→ <em>Now we create the for-loop to check each chamber.</em></p>
<p>Then we calculate the area and check if there is any detected area. If there is we check the size of this area.</p>
<pre><code>for I := 1 to Number by 1
    select_obj (Chambers, Chamber, I)
    intersection (Chamber, Pills, Pill)
    area_center (Pill, Area, Row1, Column1)

 if (Area &gt; 0)
	    min_max_gray (Pill, ImageB, 0, Min, Max, Range)

	    if (Area &lt; 3800 or Min &lt; 60)
		    concat_obj (WrongPill, Pill, WrongPill)
	    endif
   
  else
		concat_obj (MissingPill, Chamber, MissingPill)
	endif
endfor
</code></pre>
<p><strong>Step 7: Display results and statistics</strong></p>
<p>After all these calculations and writing of code it is time to showcase the results.</p>
<p>First we clear the Graphics window and display the transformed picture to display the results onto. We also set a new color to display the resulting areas.</p>
<p>→ <em>[???]</em></p>
<pre><code>dev_clear_window ()
dev_display (ImageAffineTrans)
dev_set_color ('forest green’)
</code></pre>
<p>Now we start by counting the creating objects in the previous section and display the results.</p>
<pre><code>count_obj (Pills, NumberP)
count_obj (WrongPill, NumberWP)
count_obj (MissingPill, NumberMP)
dev_display (Pills)
</code></pre>
<p>With these variables we can determine if the packaging should be accepted or rejected. Lets display this onto the screen.</p>
<p>If there are any pills missing or wrongly filled the program will display a ‘Not OK’ message, otherwise we accept the package.</p>
<p>→ <em>Copy this code to your program:</em></p>
<pre><code>if (NumberMP &gt; 0 or NumberWP &gt; 0)
    disp_message (WindowHandle, 'Not OK', 'window', 12, 12 + 600, 'red', 'true')
else
    disp_message (WindowHandle, 'OK', 'window', 12, 12 + 600, 'forest green', 'true')
endif
</code></pre>
<p>Lets also display the three variables into the picture and setup a message to show which pills are wrong if any.</p>
<p>→ <em>We first create a text for this message with the following code:</em></p>
<pre><code>Message := '# Correct pills: ' + (NumberP - NumberWP)
Message[1] := '# Wrong pills  :  ' + NumberWP
Message[2] := '# Missing pills:  ' + NumberMP
Colors := gen_tuple_const(3,'black')
</code></pre>
<p>→ <em>To highlight the wrong pills and empty spaces we use this code:</em></p>
<pre><code>if (NumberWP &gt; 0)
    Colors[1] := 'red'
endif

if (NumberMP &gt; 0)
    Colors[2] := 'red'
endif
</code></pre>
<p>→ <em>To conclude this section and display the message and the highlighting of the wrong pills we use the dev_display operator:</em></p>
<pre><code>disp_message (WindowHandle, Message, 'window', 12, 12, Colors, 'true')
dev_set_color ('red')
dev_display (WrongPill)
dev_display (MissingPill)
</code></pre>
<p>[PLAATJE3]</p>
<p><strong>Step 8: Create loop to read multiple pictures</strong></p>
<p>→ <em>To automate this and let the program loop we insert a for-loop and edit a line of code.</em></p>
<pre><code>Count := 6
for Index := 1 to Count by 1
    read_image (Image, 'blister/blister_' + Index$’02')
</code></pre>
<p>→ <em>At the end of all the code we add this:</em></p>
<pre><code>    if (Index &lt; Count)
	    disp_continue_message (WindowHandle, 'black', 'true')
    endif
	stop ()
endfor
</code></pre>
<p>Now you can press Run or F5 continuously until all the packages are checked.</p>
<h2 id="appendix">Appendix</h2>
<p>empty for now</p>
<h2 id="references">References</h2>
<p>empty for now</p>

