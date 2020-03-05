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
<p><img src="http://media.nu.nl/m/m1mxxpra124w_wd1280.jpg/normen-afschaffing-statiegeld-niet-gehaald.jpg" alt="enter image description here"></p>
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

