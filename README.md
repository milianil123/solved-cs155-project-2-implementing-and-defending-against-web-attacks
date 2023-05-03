Download Link: https://assignmentchef.com/product/solved-cs155-project-2-implementing-and-defending-against-web-attacks
<br>
In this project, you will gain experience implementing and defending against web attacks. You are given the source code for a banking web application written in Node. In Part 1 of this project, you will implement a variety of attacks. In Part 2, you will modify the Node application to defend against the attacks from Part 1.

The web application lets users manage bitbars, a new cryptocurrency. Each user is given 200 bitbars when they register for the site. They can transfer bitbars to other users using an intuitive web interface, as well as create and view user profiles.

You have been given the source code for the Bitbar application. Real web attackers generally would not have access to the source code of the target website’s server, but having the source might make finding the vulnerabilities a bit easier. <strong>You should not change any of the Node source code until Part 2.</strong>

Server details: Bitbar is powered by a collection of Node packages, including the Express.js web application framework, an SQLite database, and EJS for HTML templating.

<h1>1           Resources</h1>

<ul>

 <li>HTML, CSS, JavaScript http://www.w3schools.com/</li>

 <li>js (Bitbar uses version 9.11.1) https://nodejs.org/dist/latest-v9.x/docs/api/</li>

 <li>Express JS (The web framework that powers Bitbar in js) https://expressjs.com/en/4x/api.html</li>

 <li>EJS for HTML Templating (See .ejs files within views/) http://ejs.co/#docs</li>

 <li>XSS</li>

</ul>

http://crypto.stanford.edu/cs155/papers/CSS.pdf https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet

<ul>

 <li>SQL</li>

</ul>

http://www.w3schools.com/sql/ https://github.com/kriasoft/node-sqlite (The package Bitbar uses)

<ul>

 <li>Clickjacking Attacks and Defense http://media.blackhat.com/bh-eu-10/presentations/Stone/BlackHat-EU-2010-</li>

</ul>

Stone-Next-Generation-Clickjacking-slides

<ul>

 <li>No Explanation Needed http://stackoverflow.com/</li>

</ul>

<h1>2           Setup Instructions</h1>

You will run the Bitbar application in a Docker container that we have built for you. When the server is running, the site should be accessible at the URL http://localhost:3000.

Note also we will grade using the latest version of Firefox, and so we recommend you test your attacks in this browser so as to be safe. <strong>Note that Chrome recently introduced aggressive browser-side XSS guards, which may make some of your attacks unfeasible on this browser.</strong>

<strong>Detailed setup instructions:</strong>

<ol>

 <li>Install (and run) Docker Community Edition on your local machine<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>https://store.docker.com/search?type=edition&amp;offering=community</li>

 <li>Download and extract the starter code from the CS155 website.</li>

 <li>Navigate to the root directory and run ./build sh. This builds your Docker image and installs all necessary packages. This may take a couple of minutes, depending on your internet speed. A successful build should end in the line Successfully tagged cs155-proj2-image:latest</li>

 <li>To start the server, run ./start server.sh. Once you see</li>

</ol>

$ ./node modules/babel-cli/bin/babel-node.js ./bin/www The Bitbar application should be available in your browser at http://localhost:3000<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>.

You can close the server by pressing Ctrl + C in the terminal. <strong>The server will completely reset every time that you shut it down</strong>. To restart the server with a clean database, just run ./start server.sh once again.

<strong>Docker tips: </strong>You don’t need to familiarize yourself with Docker in order to complete this assignment. However, a few things that might prove useful:

<ul>

 <li>docker ps -a lists all of your containers.</li>

 <li>docker images lists your images.</li>

 <li>docker system prune -a deletes unused images and containers from your machine. (Do this when you’re done with the assignment if you want to save space!)</li>

 <li>The scripts build image and start server are simply one-line Docker commands to build a Docker image and spin up a temporary container from that image.</li>

 <li>The only file that is mapped from your local machine to the running Docker container is code/router.js. So if you start modifying other files and the modifications aren’t showing up, don’t worry. You may have to restart your container after modifying code/router.js for changes to take effect. If you decide you must modify another file for some reason, you must rebuild the Docker image to copy your changes into the image.</li>

 <li>The docs: https://docs.docker.com/</li>

</ul>

<h1>3           Part 1: Attacks</h1>

For the first part of the project, you will write a series of attacks against the Bitbar application. In each exercise, we describe what inputs you will need to provide to the grader, and what specific actions the grader will take using your input. At the end of this, the desired result described in the exercise should be obtained in order for you to receive credit. All of your attacks can assume that the site is accessible at the url http://localhost:3000. You may <strong>not </strong>use any external libraries. In particular, this means no jQuery.

You may use online resources, but please cite them in a file called README.txt inside your attacks directory.

<h2>3.1         Exploit Alpha: Cookie Theft</h2>

<ul>

 <li>Your solution is a URL starting with http://localhost:3000/profile?username=</li>

 <li>The grader will already be logged in to Bitbar as user1 before loading your URL, and will be on the Profile tab.</li>

 <li>Your goal is to steal the user’s Bitbar session cookie and send it to http://localhost:3000/steal_cookie?cookie=…cookie data here…</li>

 <li>When the attack is successful, the server will log the stolen cookie to the terminal output.</li>

 <li>The attack should not be visibly obvious to the user. Except for the browser location bar (which can be different), the grader should see a page that looks as it normally does when the grader visits their profile page. No changes to the site’s appearance or extraneous text should be visible. Avoiding the blue warning text stating that a user is not found is an important part of the attack. It is fine if the number of bitbars displayed or the contents of the profile are not correct (so long as they look “normal”). It’s also fine if the page looks weird briefly before correcting itself.</li>

 <li>Put your final answer in a file named txt.</li>

 <li>Hint: try adding things random text to the end of the URL. How does this change the HTML source code loaded by the browser?</li>

</ul>

<h2>3.2         Exploit Bravo: Cross-Site Request Forgery</h2>

<ul>

 <li>Your solution is a short HTML document named b.html that the grader will open using the web browser.</li>

 <li>The grader will already be logged in to Bitbar before loading your page.</li>

 <li>Transfer 10 bitbars from the grader’s account to the “attacker” account. As soon as the transfer is complete, the browser should redirect to http://crypto.stanford.</li>

</ul>

edu/cs155/ (so fast the user might not notice).

<ul>

 <li>The location bar of the browser should not contain localhost:3000 at any point.</li>

 <li>You may edit the session, but you may not directly edit the cookie.</li>

</ul>

<h2>3.3         Exploit Charlie: Session Hijacking with Cookies</h2>

<ul>

 <li>In this attack, you will have the login credentials of the user attacker, and try to impersonate user1, who has a user id of 1. The password for the user attacker is evil.</li>

 <li>Your solution is JavaScript code that can be copied and pasted into your browser’s JavaScript console when logged into the attacker It should trick the web application into thinking you are logged in as user1.</li>

 <li>Put your solution in txt. The grader will log in as attacker, copy the code to the browser console, and verify that they are now logged in as user1. To prove that the attack has worked, the grader should be able to send 10 bitbars to attacker.</li>

 <li>Hint: How does the site store its sessions?</li>

</ul>

<h2>3.4         Exploit Delta: Cooking the Books with Cookies</h2>

<ul>

 <li>Begin this attack by creating a new user. You have big plans for this account, so a starting balance of 100 bitbars is not sufficient for your intentions.</li>

 <li>Your solution is Javascript code that can be copied and pasted into your browser’s JavaScript console when logged into your new account. After pasting this code into the console, conducting a small transaction (e.g. send 1 bitbar to user1) will bump your account balance up to 1 million bitbars. The transaction should look completely valid to the innocent recipient. The goal here is to forge 1 million bitbars out of thin air without affecting the other users.</li>

 <li>Put your solution in txt. The grader will create a new account, paste this code into the browser console, and then send 1 bitbar to another user. <em>After logging out and back into the account, </em>the balance should be 1 million bitbars. The new balance must actually persist between log-in sessions.</li>

</ul>

<h2>3.5         Exploit Echo: SQL Injection</h2>

<ul>

 <li>Your solution is a malicious username that allows you to close an account that you do not have login access to.</li>

 <li>The grader will create a new user account with your provided username, click on “Close” and then confirm that he/she wants to close the current account.</li>

 <li>As a result user3 should be deleted from the database. The new user account should also be deleted to leave no trace of the attack behind. All other accounts should remain.</li>

 <li>If you mess up your user database while working on the problem, simply kill (ctrl-C) the Docker container and restart the server to reset the database.</li>

 <li>Put your final answer, a malicious username, in a file named txt.</li>

 <li>Hint: Read up on SQL injections and the WHERE clause.</li>

</ul>

<h2>3.6         Exploit Foxtrot: Profile Worm</h2>

<ul>

 <li>Your solution is a profile that, when viewed, transfers 1 bitbar from the current user to the user called attacker and replaces the profile of the current user with itself. Thus, if the attacker user changes their profile to whatever you provide in your solution, the following will happen: if user1 views attacker’s profile, 1 bitbar will be transferred from user1 to attacker, and user1’s profile will be replaced with your solution profile. Later, if user2 views user1’s profile, then 1 bitbar will be transferred from user2 to attacker, and user2’s profile will be replaced as well. Thus, your profile worm can very quickly spread to all user profiles.</li>

 <li>Submit a file named txt containing your malicious profile.</li>

 <li>Your malicious profile should include a witty message to the grader (to make grading a bit easier).</li>

 <li>To grade your attack, we will cut and paste the submitted profile into the profile of the “attacker” user and view that profile using the grader’s account. We will then view the copied profile with more accounts, checking for the transfer and replication.</li>

 <li>The transfer and application should be reasonably quick (under 15 seconds). During that time, the grader will not click anywhere.</li>

 <li>During the transfer and replication process, the browser’s location bar should remain at: http://localhost:3000/profile?username=x where x is the user whose profile is being viewed. The visitor should not see any extra graphical user interface elements (e.g. frames), and the user whose profile is being viewed should appear to have 10 bitbars.</li>

 <li>You will not be graded on the corner case where the user viewing the profile has no bitbars to send.</li>

 <li>The MySpace vulnerability may provide some inspiration.</li>

</ul>

<h2>3.7         Race conditions</h2>

<strong>Beware of Race Conditions: </strong>Depending on how you write your code, all of these attacks could potentially have race conditions that affect the success of your attacks. Attacks that fail on the grader’s browser during grading will receive less than full credit. To ensure that you receive full credit, you should wait after making an outbound network request rather than assuming that the request will be sent immediately.

<h1>4           Part 2: Defenses</h1>

Now that you understand how insecure the Bitbar platform really is, you must modify the application to defend against the attacks from Part 1. There might be more than one way of implementing each attack, so think about other possible attack methods – you will need to defend against all of them.

Do not change the site’s appearance or behavior on normal inputs. A non-malicious user should not notice anything different about your modified site.

When presented with bad inputs, your site should fail in a user-friendly way. You can sanitize the inputs or display an error message, though sanitizing is probably the more userfriendly option in most cases.

Express.js comes with a number of built-in defenses that were disabled in Part 1. These built-in defenses must remain disabled. Although in the real world it’s better to use standard, vetted defense code instead of implementing your own, we want you to get practice implementing these defenses. In particular, that means you cannot:

<ol>

 <li>use express.js to change the CORS policies that have been set in js,</li>

 <li>change any of the files in the views/ to enact stricter EJS escaping policies,</li>

 <li>or add any additional node packages beyond those provided to you in the starter code.</li>

</ol>

<strong>You are only allowed to implement your defenses in </strong>router.js<strong>. All other files must remain unchanged.</strong>

You are allowed to sanitize inputs using default JavaScript functions, but make sure you don’t over-sanitize (for example, the profile should still allow the same set of sanitized HTML tags minus any you deem unsafe).

We highly encourage use of the functions imported from ’./utils/crypto’ in your defenses. Specifically, consider how the generateRandomness and HMAC functions can be used to prevent CSRF and cookie tampering, respectively.

Finally, please describe your defenses in README.txt. A couple sentences for each exploit from Part 1 is sufficient.

<a href="#_ftnref1" name="_ftn1">[1]</a> If you are running OSX Linux, or Windows Enterprise, this should be very straightforward; just pick your operating system and download away. However, Docker for Windows does not currenly support Windows Home. You may have to download Docker Toolbox (https://docs.docker.com/toolbox/toolbox_install_ windows/), which installs Docker in a VM.

<a href="#_ftnref2" name="_ftn2">[2]</a> If you’re running Docker Toolbox, it may be available at your VM’s broadcasted IP address instead.