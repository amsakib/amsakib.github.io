---
layout: post
title: Operating Systems : Synchronization
date: 2015-05-28 20:25
author: amsakib
comments: true
categories: [Operating Systems, Study Notes]
---
<p style="text-align: justify;">মাল্টিথ্রেডিং প্রোগ্রামিং এর একটি ক্লাসিক উদাহরন হলো <a href="http://en.wikipedia.org/wiki/Producer–consumer_problem" target="_blank">Producer - Consumer</a> প্রোগ্রাম। এই প্রোগ্রামের মূল উদ্দেশ্য হলো Producer নামে একটি থ্রেড কোনো ডাটা প্রোডিউস করবে এবং একই সাথে Consumer নামক একটি থ্রেড থাকবে যা Producer এর উৎপন্ন করা ডাটা কনজিউম করবে / ভোগ করবে। এই প্রোগ্রামে buffer নামক একটি অ্যারে থাকে যেখানে Producer ডাটা জমা রাখে এবং Consumer এই অ্যারে থেকে ডাটা কনজিউম করে।  এখন এই প্রোগ্রামকে একটু মোডিফাই করা যাক। এখানে একটি counter variable নেওয়া হোক যা ডাটা প্রডিউস হলে increment হবে এবং কনজিউম হলে decrement হবে। তাহলে আমাদের Producer Process এর অংশবিশেষ হবেঃ</p>

<pre class="striped:false lang:c decode:true " title="Producer Process">while(true) {
    /* produce an item in nextProduced */
    while (counter == BUFFER_SIZE) ; /* Do NOTHING */
    buffer[in] = nextProduced; /* might be any random data */
    in = (in + 1) % BUFFER_SIZE;
    counter++;
}</pre>
<p style="text-align: justify;">এবং Consumer Process এর অংশবিশেষ হবেঃ</p>

<pre class="striped:false lang:c decode:true" title="Consumer Process">while(true) {
    while (counter == 0) ; /* Do NOTHING */
    nextConsumed = buffer[out]; /* Consumed data in nextConsumed */
    out = (out+1) % BUFFER_SIZE;
    counter--;
}</pre>
<p style="text-align: justify;">এখানে counter++ স্টেটমেন্ট মেশিন একটি রেজিস্টার ভ্যারিয়েবলে এভাবে এক্সিকিউট করতে পারেঃ</p>
<p style="text-align: justify;">register<sub>1</sub> = counter
register<sub>1</sub> = register<sub>1</sub> + 1
counter = register<sub>1</sub></p>
<p style="text-align: justify;">আবার counter-- স্টেটমেন্টি আরেকটি রেজিস্টার ভ্যারিয়েবলে এভাবে এক্সিকিউট হতে পারেঃ</p>
<p style="text-align: justify;">register<sub>2</sub> = counter
register<sub>2</sub> = register<sub>2</sub> - 1
counter = register<sub>2</sub></p>
<p style="text-align: justify;">এখানে উল্লেখ্য register<sub>1</sub> এবং register<sub>2</sub> একই CPU রেজিস্টার হতে পারে। ধরা যাক প্রথমে counter =5। এখন দেখা যাক বিভিন্ন সময়ে producer এবং consumer কি এক্সিকিউট করছেঃ</p>
<p style="text-align: justify;"><em>T<sub>0</sub>: producer  execute register<sub>1</sub> = counter          </em>[register<sub>1</sub> = 5 ]<em>
T<sub>1</sub>: producer   execute register<sub>1</sub> = register<sub>1</sub> + 1</em> [register<sub>1</sub> = 6 ]<em>
T<sub>2</sub>: consumer execute register<sub>2</sub> = counter         </em>[register<sub>2</sub> = 5 ]<em>
T<sub>3</sub>: consumer execute register<sub>2</sub> = <em>register<sub>2</sub></em>  - 1</em> [register<sub>2</sub> = 4]<em>
T<sub>4</sub>: producer  execute counter = register<sub>1</sub>          </em>[counter= 6   ]<em>
T<sub>5</sub>: consumer execute counter = register<sub>2 </sub>         </em>[counter= 4   ]<em>
</em></p>
<p style="text-align: justify;">উপরের অংশ থেকে দেখা যাচ্ছে যে একই ভ্যারিয়েবল counter এর উপর যখন একসাথে অপারেশন চলছে তখন T4 সময়ে এর মান ৬ হলেও T5 সময়ে তা হয়ে গেলো ৪ ! যদিও মাত্র ১ কমে ৫ হওয়ার কথা !!!!</p>
<p style="text-align: justify;">এরকম অবস্থায় যখন একাধিক প্রসেস একই ডাটাকে একইসাথে ব্যাবহার করে, এবং এর এক্সিকিউশনের ফলাফল যেভাবে access হয় তার উপর নির্ভর করে, তাকে <strong>RACE CONDITION</strong> [ রেস কন্ডিশন ] বলে। এই Race Condition কে প্রতিরোধ করার জন্য, আমাদেরকে এটা নিশ্চিত করতে হবে যে, একটি ভ্যারিয়েবলকে একই সময়ে দুটো প্রসেস যেনো  মডিফাই করতে না পারে। এজন্য আমাদেরকে Process Synchronization জানা প্রয়োজন।</p>

<h2 style="text-align: justify;">The Critical-Section Problem</h2>
<p style="text-align: justify;">ধরা যাক একটি সিস্টেমে n সংখ্যক প্রোসেস আছে [ P0, P1 .... Pn-1 ]। প্রতিটি প্রসেসে একটি বিশেষ কোড সেগমেন্ট থাকে। এই কোড সেগমেন্টকে <strong>Critical Section</strong> বলে। এই ক্রিটিকাল সেকশনে প্রসেস মূলত কোনো গ্লোবাল ভ্যারিয়বেলের ভ্যালু চেঞ্জ করতে পারে,  অ্যারে/টেবল আপডেট করতে পারে বা কোনো ফাইলে কোনো কিছু লিখতে পারে এবং এরকম আরও অনেক কাজ। অপারেটিং সিস্টেমের একটি গুরুত্বপূর্ণ ফিচার হলো যখন কোনো একটি প্রসেস critical section এক্সিকিউট করে তখন অন্য কোনো প্রসেসকে critical section এক্সিকিউট করতে দেয় না। অতএব,<strong> কখনও দুটো প্রসেস একসাথে Critical Section এক্সিকিউট করতে পারে না।</strong></p>
<p style="text-align: justify;"><strong>Critical Section Problem</strong> হলো একটি প্রোটোকল ডিজাইন করা, যেনো প্রসেসগুলো নিজেদের মধ্যে সহযোগীতা করে। প্রতিটি প্রসেসকে অবশ্যই ক্রিটিকাল সেকশনে যাবার জন্য পারমিশন নিতে হবে। এই পারমশনটি নিতে হয় <strong>entry section</strong> এ। ক্রিটিকাল সেকশন শেসে একটি <strong>exit section</strong> থাকতে পারে। আর বাদ বাকি কোডগুলো <strong>remainder section</strong> এ থাকবে। এরকম একটি প্রোসেসের স্ট্রাকচার দেখানো হলোঃ</p>

<pre class="lang:c++ decode:true " title="General Structure of Typical Process">do {
    /* Entry Section */
         /* critical section */
    /* Exit Section */
         /* remainder section */
}while(true);</pre>
<p style="text-align: justify;">এই Critical-Section Problem এর সমাধানকে অবশ্যই নিচের তিনটি requirement কে satisfy করতে হবেঃ</p>

<ol style="text-align: justify;">
	<li><strong>Mutual exclusion:</strong> যদি প্রসেস P1 ক্রিটিকাল সেকশনে রানিং অবস্থায় আছে, তাহলে অন্য কোনো প্রসেস তার ক্রিটিকাল অবস্থা রান করতে পারবে না।</li>
	<li><strong>Progress:</strong> যদি কোনো প্রসেস ক্রিটিকাল সেকশনে রান না করে এবং কিছু প্রসেস ক্রিটিকাল সেকশনে প্রবেশ করতে চায়, তাহলে শুধুমাত্র সেই সকল প্রসেস ক্রিটিকাল সেকশনে পরবর্তীতে কে প্রবেশ করবে তার জন্য সিদ্ধান্ত নিতে পারে, যেগুলো এক্সিকিউট হচ্ছে না এবং  যে সকল প্রসেস তাদের remainder সেকশনে নেই এবং এই সিলেকশন অসীম সময়ের জন্য চলতে পারে না।</li>
	<li><strong>Bounded waiting: </strong> একটি প্রসেস কতোবার Critical Section এ প্রবশে করার জন্য request করতে পারবে তার জন্য একটি সীমা বা লিমিট থাকবে।</li>
</ol>
<p style="text-align: justify;">অপারেটিং সিস্টেম ২ভাবে Critical Section handle করে থাকে -<strong> (১)  preemptive kernels এবং (২) non-preemptive kernels.</strong> Preemptive kernel কার্নেল মোডে প্রসেস চলতে দেয়, অপরদিকে Non-preemptive কার্নেল মোডে প্রসেস রান করতে দেয় না।</p>

<h2 style="text-align: justify;">Peterson's Solution</h2>
<p style="text-align: justify;">Critical-Section Problem এর একটি ক্লাসিক সফটওয়্যার বেজড সলিউশন হলো <a href="http://en.wikipedia.org/wiki/Peterson's_algorithm" target="_blank">Peterson's Solution</a>. ১৯৮১ সালে গ্যারি এল. পিটারসন এই ফরমুলাটি দেন। এই অ্যালগরিদমটি Critical-Section Problem এর তিনটি শর্ত ( Mutual Exclusion, Progress, Bounded Waiting ) পূর্ণ করে।</p>
<p style="text-align: justify;">Peterson's Solution মূলত দুটো প্রোসেসের মধ্যে সীমাবদ্ধ যারা critical section এবং remainder section এর মধ্যে alter করার মাধ্যমে এক্সিকিউট করে থাকে। ধরা যাক, প্রসেস দুটি হলো P1 এবং P2. কোডিং এর সুবিধার্তে যখন Pi কে চিহ্ণিত করা হবে তখন অন্য প্রসেসকে ( Pj) ডিনোট করার জন্য j = 1 -i ব্যবহার করা হবে।</p>
<p style="text-align: justify;">পিটারসন এর সমাধান দুটো ডাটা আইটেম শেয়ার করে থাকে। এগুলো হলো:</p>

<pre>int turn;
boolean flag[2];</pre>
<p style="text-align: justify;">এখানে turn ভ্যারয়েবলটি এখন কোন প্রসেস critical section এ প্রবেশ করবে তা নির্দেশ করে। অর্থাৎ যদি turn == i হয় তাহলে Pi ক্রিটিকাল সেকশনে প্রবেশ করতে পারবে। flag অ্যারেটা ব্যাবহার করা হয় কোন প্রসেস critical section এ প্রবেশ করার জন্য রেডি আছে  তা বুঝার জন্য। উদাহরনসরূপঃ যদি flag[i] এর মান true হয় তাহলে Pi প্রসেস ক্রিটিকাল সেকশনে প্রবেশ করার জন্য প্রস্তুত।</p>
<p style="text-align: justify;">Critical Section এ প্রবেশ করার জন্য Pi প্রসেসকে প্রথমে flag[i] এর মান true সেট করতে হবে এবং turn ভ্যলুকে j হিসেবে সেট করতে হবে। অন্য প্রসেসটি এই সময়ে critical section এ প্রবেশ করতে পারবে। যদি দুটি প্রসেস একই সময় critical section এ প্রবেশ করতে চায় তাহলে turn এর মান একই সাথে i ও j সেট হবে। কিন্তু এর যে কোনো একটি মান থাকবে, যেহেতু একটি ভ্যারিয়েবলের মান সবসময় overwrite হয়ে থাকে।  Peterson's Solution এর স্ট্রাকচার নিম্নরুপঃ</p>

<pre class="lang:c++ decode:true" title="Peterson's Solution">do {
    flag[i] = true;
    turn = j;
    while (flag[j] &amp;&amp; turn == j);
        /* CRITICAL SECTION */
    flag[i] = false;
        /* REMAINDER SECTION */
}while(true);</pre>
<strong>এই সলিউশনের প্রমাণ Operating System Concepts 8th Edition বইয়ের 230 পৃষ্ঠা দ্রষ্টব্য।</strong>
<h2 style="text-align: justify;">Synchronization Hardware</h2>
<p style="text-align: justify;">Critical Section Problem এর Peterson এর দেওয়া সফটওয়্যার বেজড সলিউশনটি আধুন কম্পিউটার আর্কিটেকচারে কাজ করার নিশ্চয়তা নাই। এর বদলে আমরা একটি <strong>lock</strong> নামক সিম্পল টুল ব্যবহার করতে পারি। এই lock টুলটি race condition থেকে প্রতিরোধের ব্যবস্থা করবে। যখন কোনো প্রসেস ক্রিটিকাল সেকশনে প্রবেশ করবে তখন সে lock করে রাখবে এবং যখন ক্রিটিকাল সেকশন থেকে বের হয়ে যাবে তখন lock কে release করে দিবে।</p>
<p style="text-align: justify;">Uniprocessor Environment এ খুব সহজেই Critical Section Problem সমাধান করা সম্ভব যদি shared variable মোডিফাই করার সময় interrupt ঘটা কে বাধা দেওয়া যায়। এসময় শুধু মাত্র বর্তমান সিকুয়েন্স এক্সিকিউট হবে, অন্য কোনো ইনস্ট্রাকশন রান করা যাবে না। তবে এই সমাধানটি Multiprocessor Environment এ কাজ করে না।</p>

<h2 style="text-align: justify;">Seamaphores</h2>
<p style="text-align: justify;">Semaphore হলো একটি integer variable S, যা initialization ছাড়া শুধুমাত্র wait() এবং signal() অপারেশনের সময় ব্যবহার করা যায়। Wait() অপারেশনকে P দ্বরা প্রকাশ করা হয় এবং Signal() অপারেশনকে V দ্বারা প্রকাশ করা হয়। wait() অপারেশনের ডেফিনিশন হলোঃ</p>

<pre class="lang:c++ decode:true" title="wait() operation">wait(S) {
    while( S &lt;= 0 ) ; // No Operation
    S--;
}</pre>
<p style="text-align: justify;"> signal() অপারেশনের ডেফিনিশন হলোঃ</p>

<pre class="lang:c++ decode:true " title="signal() operation">signal(S) {
    S++;
}</pre>
<p style="text-align: justify;">এই সিমাফোর ভ্যালুর মোডিফিকেশন অবশ্যই wait অথবা signal ফাংশনের মধ্যে আলাদাভাবে ( individually ) হতে হবে। অর্থাৎ, যখন একটি প্রসেস এই semaphore এর মান পরিবর্তন করবে, তখন অন্য কোনো প্রসেস একই সময়ে এর মান পরিবর্তন করতে পারবে না।</p>
<p style="text-align: justify;">Semaphore কে C প্রোগ্রামিং এর স্ট্রাকচার ব্যবহার করে এভাবে ডিফাইন করা যায়ঃ</p>

<pre class="lang:c decode:true " title="Semaphore">typedef struct {
    int value;
    struct process *list;
} semaphore;</pre>
<p style="text-align: justify;">প্রতিটি semaphore এ একটি integer value থাকবে এবং একটি process list থাকবে। যখন একটি প্রসেসকে অবশ্যই semaphore এ অপেক্ষা করতে হবে তখন তাকে অবশ্যই process এর লিস্টে অ্যাড করতে হবে। আবার signal() অপারেশনের সময় process list থেকে তাকে মোছা যাবে। তাহলে wait() semaphore অপারেশন হবেঃ</p>

<pre class="lang:default decode:true" title="wait() semaphore operation">wait(semaphore *S) [
    S-&gt;value--;
    if(S-&gt;value &lt; 0 ) {
        // add this process to S-&gt;list;
        block();
    }
}</pre>
<p style="text-align: justify;"> এবং signal() অপারেশন হবেঃ</p>

<pre class="lang:default decode:true " title="signal() semaphore operation">signal(semaphore *S) [
    S-&gt;value++;
    if(S-&gt;value &lt;= 0 ) {
        // remove a process P from S-&gt;list;
        wakeup(P);
    }
}</pre>
<h2>Deadlocks and Starvation</h2>
<p style="text-align: justify;">Waiting queue এর মাধ্যমে semaphore implement করতে গেলে এমন একটি পরিস্থিতির সৃষ্টি হতে পারে যখন দুই বা ততোধিক প্রসেস অসীম সময়ের জন্য এদের যে কোনো একটি প্রসেসেরর ইভেন্টের জন্য অপেক্ষা করতে থাকবে। যখন এ ধরনের পরিস্থিতির সৃষ্টি হয় তখন একে বলা হয় <strong>Deadlocked.</strong></p>
<p style="text-align: justify;">Deadlock বোঝার জন্য ধরা যাক দুটি প্রসেস P0 এবং P1 রয়েছে। প্রতিটিই ২টি সিমাফোর S এবং Q ব্যবহার করছে এবং এদের মান 1.</p>

<table>
<thead>
<tr>
<th style="text-align: center;">P<sub>0</sub></th>
<th style="text-align: center;">P<sub>1</sub></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">wait(S);</td>
<td style="text-align: center;">wait(Q);</td>
</tr>
<tr>
<td style="text-align: center;">wait(Q);</td>
<td style="text-align: center;">wait(S);</td>
</tr>
<tr>
<td style="text-align: center;">.</td>
<td style="text-align: center;">.</td>
</tr>
<tr>
<td style="text-align: center;">.</td>
<td style="text-align: center;">.</td>
</tr>
<tr>
<td style="text-align: center;">signal(S);</td>
<td style="text-align: center;">signal(Q);</td>
</tr>
<tr>
<td style="text-align: center;">signal(Q);</td>
<td style="text-align: center;">signal(S);</td>
</tr>
</tbody>
</table>
<p style="text-align: justify;">ধরা যাক, P0, wait(S) এক্সিকিউট করছে এবং P1, wait(Q) এক্সিকিউট করছে। যখন P0, wait(Q) এক্সিকিউট করবে তখন একে অবশ্যই P1 এর signal(Q) এক্সিকিউট না করা পর্যন্ত অপেক্ষা করতে হবে। এবই ভাবে P1 যখন wait(S) এক্সিকিউট করতে চাইবে তখন একে অবশ্যই signal(S) সম্পন্ন হওয়া পর্যন্ত অপেক্ষা করতে হবে। কিন্তু যেহেতু signal অপারেশনগুলো এক্সিকিউট হতে পারছে না সেহেতু বলা যায় P0 ও  P1 ডেডলকড (Deadlocked) অবস্থায় আছে।</p>
<p style="text-align: justify;">Deadlock এর সাথে সম্পর্কযুক্ত আরও একটি সমস্যা আছে যেটাকে বলা হয় <strong>indefinite blocking</strong>, or <strong>starvation</strong>. এটি তখনই ঘটে থাকে যখন একটি প্রসেস তার নিজের সিমাফোরের মধ্যে অনির্দিষ্ট সময়ের জন্য অপেক্ষা করে।</p>

<h2 style="text-align: justify;">The Dining-Philosophers Problem</h2>
<p style="text-align: justify;">এই সমস্যাটা আমার কাছে একটি মজার সমস্যা মনে হয়েছে। এই প্রবলেমে ৫ জন দার্শনিকের কথা বলা হয়েছে যারা সারা জীবন শুধু চিন্তা করে আর খাওয়া দাওয়া করে সময় পার করে। এই দার্শনিকরা একিট গোল টেবিলকে ঘিরে বসে এবং এদের প্রত্যেকের জন্য একটি করে চেয়ার আছে। টেবিলের মাঝখানে একটি বাটিতে ভাত আছে এবং টেবিলে ৫ টা চপস্টিক আছে। যখন কোনো দার্শনিক চিন্তা করে তখন সে তার সাথীদের সাথে কোনো প্রকার কথা বলে না। কিন্তু সময়ের সাথে সাথে  যখন কোনো এক দার্শনিকের ক্ষুধা লাগে তখন সে তার দু'পাশের ২টি চপস্টিক নেওয়ার চেষ্টা করে। একক সময়ে একজন দার্শনিক শুধুমাত্র একটি চপস্টিক নিতে পারবে। অবশ্যই সে এমন কোনো চপস্টিক নিতে পারবে না যা ইতমধ্যেই অন্য কোনো দার্শনিক নিয়ে রাখছে। যখন একজন ক্ষুধার্ত দার্শনিকের হাতে ২টি চপস্টিকই চলে আসবে তখন সে ভাতের বাটি থেকে ভাত খেতে থাকবে। যখন তার খাওয়া শেষ হবে, তখন সে চপস্টিকগুলো আগের অবস্থানে রেখে দিবে এবং  আবার চিন্তা করা শুরু করে দিবে :D</p>


[caption id="" align="aligncenter" width="305"]<img class="" src="http://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter5/5_13_DiningPhilosophers.jpg" alt="The Dining-Philosopher Problem Situation" width="305" height="293" /> The Dining-Philosopher Problem Situation[/caption]
<p style="text-align: justify;">এই <a href="http://en.wikipedia.org/wiki/Dining_philosophers_problem" target="_blank">Dining-Philosopher Problem</a> টাকে একটি ক্লাসিক সিন্ক্রোনাইজেশন প্রবলেম হিসেবে ধরা হয়। এই প্রবলেমের একটা সহজ সমাধান হলো প্রতিটি চপস্কিটকে এক একটি সিমাফোর হিসেবে রিপ্রেজেন্চ করা। একজন দার্শনিক wait() অপারেশন এক্সিকিউশনের মাধমে একটি চপস্টিক নিবে এবং signal() অপারেশনের মাধ্যমে এই চপস্টিক খাবার শেষে ছেড়ে দিবে। তাহলে শেয়ার কৃত ডাটা হবেঃ</p>

<pre style="text-align: left;" class="">semaphore chopstick[5];</pre>
<p style="text-align: justify;">এখানে শুরুতে সব এলিমেন্টের মান থাকবে 1. একজন দার্শনিকের জন্য কোডের সাধারণ স্ট্রাকচার হবে এরকমঃ</p>

<pre class="lang:c++ decode:true " title="The structure of philospher i">do {
    wait(chopstick[i]);
    wait(chopstick[(i+1) % 5]);
    /* ------------
           EAT
       ------------ */
    signal(chopstick[i]);
    signal(chopstick[(i+1) % 5]);
    /* ------------
           Think : কোনো কাম নাই হালাগো
       ------------ */
}while(true);</pre>
<p style="text-align: justify;">যদিও এই সমাধান নিশ্চিত করে যে কখনও ২ জন পাশাপাশি দার্শনিক একসাথে খেতে পারবে না তারপরও এটি Deadlock এর সৃষ্টি করতে পারে। মনে করি, সকল দার্শনিকের এক সাথে ক্ষুধা লাগলো এবং তারা প্রত্যেকে তাদের বামপাশের চপস্টিক নিলো। তাহলে কি একটাও চপস্টিক ফাঁকা থাকলো ? সবগুলো চপস্টিকের মান এখন হবে 0 ফলে ডানদিকের চপস্টিক নেওয়ার জন্য প্রত্যেককে আজীবনের জন্য অপেক্ষা করতে হবে ( ;) ) । Deadlock প্রতিহত করার জন্য নিচের বিষয়গুলো বিবেচনা করা যেতে পারেঃ</p>

<ul style="text-align: justify;">
	<li>কখনও একসাথে ৪ জনের বেশি দার্শনিককে খেতে দেওয়া যাবে না !</li>
	<li>একজন দার্শনিক তখনই চপস্টিক ওঠাতে পারবে যখন তার দুপাশের চপস্টিক ফাঁকা থাকবে। [ ক্রিটিকাল সেকশনে তাকে এই কাজ করতে হবে। ]</li>
	<li>Asymmetric Solution: বেজোড় দার্শনি প্রথমে তার বাম পাশের চপস্টিক নিবে তারপর তার ডান পাশের চপস্টিক নিবে।  অন্যদিকে জোড় দার্শনিক তার ডানদিকের চপস্টিক প্রথমে নিবে তারপর বামদিকের চপস্টিক নিবে।</li>
</ul>
<h2 style="text-align: justify;">Monitors</h2>
<p style="text-align: justify;">Monitor হলো একটি Abstract Data Type. এটি একটি কন্সট্রাক্ট যেখানে কিছু ভ্যারিয়েবল এবং প্রসিডিউর থাকতে পারে। [ একটু খেয়াল করে দেখুন এটি ঠিক Object Oriented Programming এর Class এর মতো ]. Monitor type এর রিপ্রেজেন্টশন সরাসরি বিভিন্ন প্রসেস ব্যবহার করতে পারে না। তাই একটি Monitor এর মধ্যে ডিফাইন করা ভ্যারিয়েবলগুলো শুধু ঐ মনিটরের ভিতরের Procedure ( Function ) গুলো ব্যবহার করতে পারবে। একই ভাবে মনিটেরর ভেতর একটি ফাংশনের লোকাল ভ্যারিয়েলগুলো অন্য কোনো ফাংশন ব্যবহার করতে পারবে না। একটি Monitor এর স্ট্রাকচার নিম্নরূপঃ</p>

<pre class="lang:default decode:true " title="Syntax of a Monitor">monitor monitor_name {
    // shared variable declarations
    procedure P1(.....) {
         .....
    }
    procedure P2(.....) {
         .....
    }
         .
         .
         .
    procedure Pn(.....) {
         .....
    }
}</pre>
<p style="text-align: justify;">এই মনিটর এটা নিশ্চিত করে যে, একই সময়ে শুধুমাত্র একটি প্রসেস মনিটরের মধ্যে সক্রিয় থাকতে পারে। মনিটরের ভেতর condition নামে আরও একটি কন্সট্রাক্ত থাকে। যেই কন্ডিশনের মধ্যে শুধু wait() এবং signal() অপারেশন থাকবে।</p>

<pre class="lang:default decode:true " title="conditon construction">condition x, y;
x.wait();
x.signal();</pre>
<h3>The Dining-Philosopher Problem Solution using Monitors</h3>
<p style="text-align: justify;">Monitor ব্যবহার করে Dining-Philosopher Problem এর জন্য Deadlock Free সমাধান সম্ভব। এই সমাধান নিশ্চিত করে যে একজন দার্শনিক তখনই চপস্টিক নিবে যখন তার দু'পাশের চপস্টিক অব্যবহৃত আছে। এজন্য ৩টি স্টেট ব্যবহার করা হয়। যা enum ডাটা স্ট্রাকচারের মাধ্যমে এভাবে দেখানো যেতে পারে।</p>

<pre class="" style="text-align: justify;">enum {THINKING, HUNGRY, EATING} state[5];</pre>
<p style="text-align: justify;">Philosopher i তার স্টেট state[i] = EATING সেট করতে পারে যদি তার প্রতিবেশী দুজনের state EATING না হয় অর্থাৎ, ( state[(i+1)%5] != EATING ] ) এবং ( state[(i-1)%5)] != EATING ) .</p>
<p style="text-align: justify;">এছাড়াও আমাদের কে কন্ডিশন ডিক্লেয়ার করতে হবেঃ</p>

<pre class="" style="text-align: justify;">condition self[5];</pre>
<p style="text-align: justify;">সম্পূর্ণ সলিউশনটি নিম্নরূপ হবেঃ</p>

<pre class="lang:default decode:true " title="Monitor Solution to the Dining-Philosopher Problem">monitor dp
{
    enum {THINKING, HUNGRY, EATING} state[5];
    condition self[5];
    
    void pickup(int i) {
        state[i] = HUNGRY;
        test(i);
        if(state[i] != EATING) {
            self[i].wait();
        }
    }

    void putdown(int i) {
        state[i] = THINKING;
        test((i+4)%5);
        test((i+1)%5);
    }

    void test(int i) {
        if((state[(i+4)%5] != EATING) &amp;&amp; (state[i] == HUNGRY) &amp;&amp; 
            (state[(i+1)%5] != EATING)){
          state[i] = EATING;
            self[i].signal();
        }
    }

    void initialization_code() {
        for(int i=0; i&lt;5; i++)
            state[i] = THINKING;
    }
}</pre>
<p style="text-align: justify;">উপরের সমাধানে pickup ফাংশন একটি চপস্টিক উঠাবে এবং putdown ফাংশন চপস্টিক রাখবে। এখানে test() ফাংশনের মাধ্যমে chopstick available কিনা তা পরীক্ষা করা হয়েছে।</p>
<p style="text-align: justify;"></p>
