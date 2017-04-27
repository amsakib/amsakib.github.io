---
layout: post
title: Operating Systems : Process
date: 2015-05-29 20:34
author: amsakib
comments: true
categories: [Operating Systems, Study Notes]
---
<p style="text-align: justify;">সাধারণত একটি প্রসেস হলো একটি প্রোগ্রাম এক্সিকিউশন। তবে প্রসেস শুধুমাত্র প্রোগ্রাম কোড ছাড়াও আরও অনেক কিছু বোঝায়, এটিকে অনেক সময় <strong>Text Section</strong> বলা হয়। এছাড়াও একটি প্রসেসে বর্তমান অ্যকটিভিটি একটি ইন্টিজার ভ্যালু হিসেবে থাকে, একে <strong>Program Counter</strong> বলে। এছাড়াও একটি প্রসেসে <strong>Stack</strong> নামক টেম্পরারি জায়গা থাকে, যেখানে বিভিন্ন ফাংশন প্যারামিটার, রিটার্ন ভ্যালু ও লোকাল ভ্যারিয়েবলের মান স্টোর থাকে। প্রসেসে গ্লোবাল ভ্যারিয়েবল স্টোর করে রাখার জন্য<strong> Data Section</strong> থাকে। এছাড়াও রানটাইমে ডায়নামিকালি অ্যালোকেট করার জন্য <strong>heap section</strong> থাকতে পারে। একটি প্রসেসের Memory স্ট্রাকচার তাহলে নিম্নরূপঃ</p>


[caption id="" align="aligncenter" width="237"]<img class="" src="http://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_01_Process_Memory.jpg" alt="Process in Memory" width="237" height="381" /> Process in Memory[/caption]
<h2 style="text-align: justify;">Process State:</h2>
<p style="text-align: justify;">Process এক্সিকিউশনের সাথে সাথে এর বিভিন্ন স্টেটের পরিবর্তন হয়ে থাকে। একটি প্রসেস নিচের যে কোনো একটি স্টেটে থাকতে পারেঃ</p>


[caption id="" align="aligncenter" width="652"]<img class="" src="http://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_02_ProcessState.jpg" alt="Diagram of Process State" width="652" height="254" /> Diagram of Process State[/caption]
<ul style="text-align: justify;">
	<li><strong>New:</strong> প্রসেসটি কেবল তৈরী হয়েছে/হচ্ছে।</li>
	<li><strong>Running:</strong> প্রসেসটি এক্সিকিউট হচ্ছে।</li>
	<li><strong>Waiting:</strong> প্রসেসেটি কোন একটি ইভেন্ট ঘটার জন্য অপেক্ষা করছে (এটা হতে পারে কোনো I/O completion অথবা reception of Signal)</li>
	<li><strong>Ready:</strong> প্রসেসটি প্রসেসর দ্বারা এক্সিকিউট হবার জন্য অপেক্ষা করছে।</li>
	<li><strong>Terminated:</strong> প্রসেসটি তার এক্সিকিউশন শেষ করেছে।</li>
</ul>
<h2 style="text-align: justify;">Process Control Block (PCB)</h2>
<p style="text-align: justify;">Operating System এ প্রতিটি প্রসেস Process Control Block (PCB) বা Task Control Block এর মাধ্যমে রিপ্রেজেন্ট করা হয়ে থাকে। প্রসেস কন্ট্রোল ব্লকে (PCB)তে একটি প্রসেস সংক্রান্ত বিভিন্ন তথ্য থাকে। একটি PCB অনেকটা নিম্নরূপঃ</p>
<p style="text-align: justify;">PCB তে প্রসেস সম্পর্কে যে সকল তথ্য থাকতে পারেঃ</p>

<ul style="text-align: justify;">
	<li><strong>Process State:</strong> একটি প্রসেস কোন স্টেটে আছে ( New/Running/Waiting/Read/Terminated) তা এই অংশে থাকবে।</li>
	<li><strong>Program Counter: </strong>প্রসেসটি পরবর্তীতে কোন ইন্সট্রাকশন এক্সিকিউট করবে তার address এই কাউন্টারে থাকে।</li>
	<li><strong>CPU Registers: </strong>Register এর সংখ্যা ও টাইপ কম্পিউটার আর্কিটেকচার অনুসারে বিভিন্ন প্রকারের হতে পারে। যেমনঃ Accumulators, index registers, stack pointers এবং general-purpose registers।</li>
	<li><strong>CPU Scheduling Information: </strong>বিভিন্ন প্রসেসের priority, scheduling queue র পয়েন্টার সহ অন্যান্য Scheduling Parameter এর তথ্য এখানে থাকতে পারে।</li>
	<li><strong>Memory-management Information: </strong>এখানে base অথবা limit register, page table বা segment table এ বিভন্ন তথ্য থাকতে পারে।</li>
	<li><strong>Accounting Information: </strong>এখানে CPU কতক্ষন ব্যবহার হয়েছে, time limits, job অথবা process numbers এর মত তথ্য থাকতে পারে।</li>
	<li><strong>I/O status Information: </strong>এখানে কোন প্রসেসে কোন I/O device ব্যবহার করছে, কোন কোন ফাইল ওপেন আছে এই ধরনের তথ্য থাকবে।</li>
</ul>
<h3 style="text-align: justify;"> CPU Switch from Process to Process:</h3>
[caption id="" align="aligncenter" width="640"]<img class="" src="http://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_04_ProcessSwitch.jpg" alt="Diagram showing CPU switch from process to process" width="640" height="523" /> Diagram showing CPU switch from process to process[/caption]
<h2 style="text-align: justify;">Threads</h2>
<p style="text-align: justify;">সাধারণত প্রোসেস হলো একটি প্রোগ্রাম যা মূলত single thread এর এক্সিকিউশন। যেমন, যখন আমরা word processing software ব্যবহার করে থাকি তখন আমরা একই সাথে অনেকগুলো থ্রেড/প্রসেস রান করাতে পারি না। আমরা একই সাথে টাইপ করার সাথে সাথে একই থ্রেডে Spell Checker রান করতে পারি না। তবে বর্তমানে অনেক প্রসেসর Multiple-Thread হ্যান্ডেল করতে পারে। সেই সকল ক্ষেত্রে PCB তে প্রতিটি থ্রেডের জন্য তথ্য জমা থাকবে।</p>

<h2 style="text-align: justify;">Process Scheduling</h2>
<p style="text-align: justify;">Multiprogramming এর উদ্দেশ্য হলো সব সময় যেনো প্রসেস রান করে থাকে যেনো CPU Utilization সর্বোচ্চ পরিমান হয়। আবার Time Sharing এর উদ্দেশ্য হলো CPU যেনো বারবার প্রসেস পরিবর্তন করে যেনো ব্যবহারকারি  প্রতিটি প্রগ্রামের সাথে খুব দ্রুত কাজ করতে পারে। এই সকল উদ্দেশ্য সফল করার জন্য <strong>Process Scheduler</strong> রয়েছে। Process Scheduler এর কাজ হলো যে সকল প্রসেস available আছে তাদের মধ্য থেকে এক্সিকিউশন করার জন্য প্রসেস সিলেক্ট করা। Single-processor system এ একই সময়ে দুটি প্রসেস রান করতে পারবে না, তাই একটি প্রসেস শেষ না হওয়া পর্যন্ত অন্য প্রসেসগুলোকে অপেক্ষা করতে হবে। CPU ফাঁকা হলেই অন্য প্রসেসগুলো rescheduled হবে।</p>

<h3 style="text-align: justify;">Scheduling Queues</h3>
<p style="text-align: justify;">Process Schedule এর জন্য মূলত তিন ধরনের Queue রয়ছে। এগুলো হলোঃ</p>

<ol style="text-align: justify;">
	<li><strong>Job Queue:</strong> কোনো প্রসেস সিস্টেমে প্রবেশ করার সাথে সাথেই তা Job Queue তে জমা হয়। মূলত Job Queue তে সিস্টেমের সকল প্রকার প্রসেস থাকে।</li>
	<li><strong>Ready Queue:</strong> যখন কোনো প্রসেস মেমোরিত থাকে এবং ready state এ থাকে ও এক্সিকিউট করার জন্য অপেক্ষমান তখন সেই প্রসেস যে লিস্টে থাকে তাকে Ready Queue বলে। এই কিউ সাধারনত লিংকড লিস্ট দিয়ে ইম্প্লিমেন্ট করা হয়। Ready Queue এর হেডারে PCB র প্রথম এবং  PCBর শেষ পয়েন্টার থাকে। প্রতিটি PCB তে একটি পয়েন্টার থাকে যা read queue এর পরবর্তী PCB কে পয়েন্ট করবে।</li>
	<li><strong>Device Queue:</strong> যে লিস্টে প্রসেসগুলো I/O Device এর জন্য অপেক্ষা করে তাকে Device Queue বলে। যেমন একটি প্রসেসের হার্ড ডিস্ক অ্যাকসেস করার প্রয়োজন হলো। কিন্তু হার্ড ডিস্কটি অন্য একটি প্রসেস দ্বারা ব্যবহৃত হচ্ছে। তখন তো এই প্রসেসকে অপেক্ষা করতে হবে হার্ড ডিস্ক অ্যাকসেস করার জন্য। তখনই সে Device Queue তে অপেক্ষমান থাকবে।</li>
</ol>
[caption id="" align="aligncenter" width="560"]<img class="" src="http://www.tutorialspoint.com/operating_system/images/queuing_diagram.jpg" alt="Queuing-diagram representation of Process Scheduling" width="560" height="402" /> Queuing-diagram representation of Process Scheduling[/caption]
<p style="text-align: justify;">একটি প্রসেস প্রথমে Ready Queue তে থাকে। এটি execution এর জন্য সিলেক্ট না হওয়া পর্যন্ত বা dispatched না হওয়া পর্যন্ত কিউতে অপেক্ষা করে। যখন প্রসেসটি CPU দ্বারা এক্সিকিউশন চলে তখন নিচের যে কোনো একটি ঘটনা ঘটতে পারেঃ</p>

<ul style="text-align: justify;">
	<li>প্রসেসটি কোনো I/O অপারেশনের জন্য অপক্ষা করতে পারে এবং I/O বা Device Queue তে যেতে পারে।</li>
	<li>প্রসেসটি কোনো নতুন সাবপ্রসেস তৈরী করতে পারে এবং ঐ সাবপ্রসেসটি শেষ না হওয়া পর্যন্ত অপেক্ষমান থাকতে পারে।</li>
	<li>CPU জোড় করে প্রসেসটিকে মুছে ফেলতে পারে। এমনটি interrupt এর কারনে ঘটতে পারে এবং এই সময় সে ready queue তে চলে যেতে পারে।</li>
</ul>
<h3 style="text-align: justify;">Schedulers</h3>
<p style="text-align: justify;">বিভিন্ন প্রকাল সিডিউলারের (<strong>scheduler</strong>) মাধ্যমে অপারেটিং সিস্টেম প্রসেস সিলেক্ট করে থাকে। মূলত দু'ধরনের scheduler রয়েছে। (১) <strong>short-term scheduler বা CPU scheduler</strong> এবং (২) <strong>long-term scheduler বা job scheduler.</strong></p>
<p style="text-align: justify;"><strong>Short-Term Scheduler:</strong> এই সিডিউলারের কাজ হলো রেডি কিউ থেকে প্রসেস সিলেক্ট করা এবং সেই প্রসেসের জন্য CPU অ্যালোকেট করে দেওয়া।</p>
<p style="text-align: justify;"><strong>Long-Term Scheduler:</strong> এই সিডিউলারের কাজ হলো Ready Queue তে প্রসেস নিয়ে আসা এবং তাদেরকে মেমোরিতে লোড করা।</p>
<p style="text-align: justify;">এই দুটি ছাড়াও <strong>Medium-Term Scheduler</strong> আছে। এই সিডিউলারের কাজ হলো যে কোনো সময় প্রসেসকে মেমোরি থেকে মুছে ফেলে degree of multiprogramming কমিয়ে দেওয়া এবং পরবর্তীতে প্রয়োজন হলে ঐ প্রসেসকে পুনরায় মেমোরিতে রেস্টোর করা।</p>
<p style="text-align: justify;">কাজরে ভিত্তিতে প্রসেসকে ২ ভাগে ভাগ করা যায়ঃ</p>
<p style="text-align: justify;"><strong>(১) I/O Bound Process:</strong> এই সকল প্রসেস তার লাইফ টাইমে বেশিরভাগ সময়ই I/O operation পারফর্ম করে থাকে। এরা খুব কমই কম্পিউটেশনের কাজ করে থাকে।</p>
<p style="text-align: justify;"><strong>(২) CPU Bound Process:</strong> এই সকল প্রোসেস I/O Bound প্রসেসের ঠিক বিপরীত। অর্থাৎ এর I/O Operation এর চেয়ে কম্পিউটেশনের কাজই বেশি করে থাকে।</p>

<h2 style="text-align: justify;">Operation on Process: Process Creation</h2>
<p style="text-align: justify;">বিভিন্ন ভাবে একটি প্রসেস তৈরী করা যেতে পারে, হয় সিস্টেমের create-process ফাংশনের মধ্য দিয়ে অথবা একটি প্রসেস চলাকালীন সময়ে। যে প্রসেস একটি নতুন প্রসেস তৈরী করে তাকে <strong>Parent Process</strong> বলা হয় এবং নতুন যে প্রসেস তৈরী হয় তাকে ঐ প্যারেন্ট প্রসেসের <strong>Children Process</strong> বলা হয়। আবার প্রতিটি children process আরও অনেক প্রসেস তৈরী করতে পারে। এভাবে একটি প্রসেস Tree গঠন হতে পারে। সাধারণত প্রতিটি প্রসেসের জন্য একটি করে id দেওয়া থাকে ( pid ), যেটা ইউনিক ইন্টিজার নাম্বার হয়ে থাকে।</p>
<p style="text-align: justify;">যখন একটি প্রসেস নতুন একিট প্রসেস তৈরী করে তখন এক্সিকিউশনের টার্ম অনুসারে দুটি সম্ভাবনা থাকেঃ</p>
<p style="padding-left: 30px; text-align: justify;">১. Parent প্রসেস children প্রসেসের সাথে পাশাপাশি এক্সিকিউট হবে।
২. Parent Process, কিছু বা সব children process এর এক্সিকিউট শেষ না হওয়া পর্যন্ত অপেক্ষা করবে।</p>
<p style="text-align: justify;">স্পেসের টার্ম অনুসারেও দুটি সম্ভাবনা থাকেঃ</p>
<p style="padding-left: 30px; text-align: justify;">১. Child Process টি তার Parent Process এর অনুরূপ (duplicate) হবে [ এক্ষেত্রে parent process এর অনুরূপ program এবং data থাকবে ]
২. Child Process এ সম্পূর্ণ নতুন একটি প্রোগ্রাম লোড হবে।</p>
<p style="text-align: justify;">UNIX অপারেটিং সিস্টেমের ক্ষেত্রে প্রসেস তৈরী করার একটি প্রোগ্রাম নিচে দেওয়া হলোঃ</p>

<pre class="lang:c decode:true" title="Creating a separate process using the UNIX fork() system call.">#include &lt;sys/types.h&gt;
#include &lt;stdio.h&gt;
#include &lt;unistd.h&gt;
int main()
{
    pid t pid;
    /* fork a child process */
    pid = fork();
    if (pid &lt; 0) { /* error occurred */
        fprintf(stderr, "Fork Failed");
        return 1;
    }
    else if (pid == 0) { /* child process */
        execlp("/bin/ls","ls",NULL);
    }
    else { /* parent process */
        /* parent will wait for the child to complete */
        wait(NULL);
        printf("Child Complete");
    }
    return 0;
}</pre>
<p style="text-align: justify;">Windows অপারেটিং সিস্টেমের ক্ষেত্রে প্রসেস তৈরী করার প্রোগ্রাম নিচে দেওয়া হলোঃ</p>

<pre class="lang:c decode:true " title="Creating a separate process using the Windows API">#include &lt;stdio.h&gt;
#include &lt;windows.h&gt;
int main(VOID)
{
    STARTUPINFO si;
    PROCESS INFORMATION pi;
    /* allocate memory */
    ZeroMemory(&amp;si, sizeof(si));
    si.cb = sizeof(si);
    ZeroMemory(&amp;pi, sizeof(pi));
    /* create child process */
    if (!CreateProcess(NULL, /* use command line */
        "C:\\WINDOWS\\system32\\mspaint.exe", /* command */
        NULL, /* don’t inherit process handle */
        NULL, /* don’t inherit thread handle */
        FALSE, /* disable handle inheritance */
        0, /* no creation flags */
        NULL, /* use parent’s environment block */
        NULL, /* use parent’s existing directory */
        &amp;si,
        &amp;pi))
        {
            fprintf(stderr, "Create Process Failed");
            return -1;
        }
    /* parent will wait for the child to complete */
    WaitForSingleObject(pi.hProcess, INFINITE);
    printf("Child Complete");
    /* close handles */
    CloseHandle(pi.hProcess);
    CloseHandle(pi.hThread);
}</pre>
<h2 style="text-align: justify;">Operation on Process: Process Termination</h2>
<p style="text-align: justify;">যখন একটি প্রসেস তার শেষ স্টেটমেন্টটি এক্সিকিউশন করে এবং exit() স্টেটমেন্টের মাধ্যমে অপারেটিং সিস্টেমের কাছ থেকে ডিলিট করতে বলে তখন সে Terminate হয়। এই সময়ে প্রসেসটি তার Parent প্রসেসকে কোনো স্ট্যাটাস ভ্যালু ( সাধারণত integer ) রিটার্ন করতে পারে। এসময় অপারেটিং সিস্টেম সমস্ত প্রকার রিসোর্স deallocate করে ফেলে।</p>
<p style="text-align: justify;">প্যারেন্ট প্রসেস চাইলে যে কোনো সময় তার Child Process কে terminate করতে পারবে। এছাড়াও ইউজার চাইলে যে কোনো সময় জোর করে কোনো প্রোসেসকে টারমিন্টে করতে পারে। নিচের কারণগুলোর কারনে সাধারনত Parent Process তার Child Process কে terminate করেঃ</p>

<ul style="text-align: justify;">
	<li>যখন Child Process কে অ্যালোকেট করে দেওয়া রিসোর্সের চেয়ে অতিরিক্ত রিসোর্স ব্যবহার করে।</li>
	<li>Child Process কে যে কাজ করতে দেওয়া হয়েছিলো তার আর প্রয়োজন নেই।</li>
	<li>যখন Parent Process শেষ ( exit ) হচ্ছে তখন অপারেটিং সিস্টেম এর child process গুলোকে Parent Terminate হওয়ার পর আর এক্সিকিউট করতে দেয় না।</li>
</ul>
<h2 style="text-align: justify;">Interprocess Communication</h2>
<p style="text-align: justify;">অপারেটিং সিস্টেমে একসাথে এক্সিকিউট হওয়া প্রসেসগুলোকে Independent Process অথবা Cooperating Process এই দুই ভাগে ভাগ করা যায়। যে সকল প্রোসেস অন্য কোনো প্রসেস দ্বারা প্রভাবিত হয় না এবং অন্য কোনো প্রসেসর সাথে কোনো প্রকার ডাটা শেয়ার করে না তাদেরকে Independent Process বলে। অপরদিকে, যে সকল প্রোসেসে অন্য কোনো প্রসেস দ্বারা প্রভাবিত হতে পারে অথবা, অন্য কোনো প্রসেসের সাথে ডাটা শেয়ার করে তাদেরকে Cooperating Process বলে।</p>
<p style="text-align: justify;">Cooperating Process কে বৈধ করার কয়েকটি কারণ হলোঃ</p>

<ul style="text-align: justify;">
	<li><strong>Information Sharing:</strong> অনেক সময়ই অনেকগুলো প্রসেসের একই ধরনের তথ্যের প্রয়োজন হতে পারে। এই ধরনের পরিস্তিতিতে এরকম পরিবেশের ব্যবস্থা করা উচিত।</li>
	<li><strong>Computation speedup:</strong> যদি আমরা কোনো টাস্কের স্পীড বৃদ্ধি করতে চাই তাহলে এটিকে বিভিন্ন সাবটাস্কে ভাগ করে সমান্তরালে এক্সিকিউট করলে computation এর স্পীড বৃদ্ধি পাবে। এসকল ক্ষেত্রে রিসোর্স শেয়ার করা প্রয়োজন।</li>
	<li><strong>Modularity</strong>: অনেক সময়ই আমাদেরকে প্রোসেস বা থ্রেডগুলোকে modular fashion  এ গঠন করা প্রয়োজন হতে পারে।</li>
	<li><strong>Convenience:</strong> ব্যবহারকারী একই সময়ে বিভিন্ন কাজ করতে পারে। যেমন editing, painting, compiling ইত্যাদি।</li>
</ul>
<p style="text-align: justify;">Cooperating Process গুলোর জন্য Interprocess Communication (IPC) মেকানিজমের প্রয়োজন। এই মেকানিজমের কাজ হলো ডাটা ও ইনফরমেশন শেয়ার করা। Interprocess Communication এর ২ টি প্রধান মডেল রয়েছে। এগুলো হলোঃ (১) <strong>shared memory</strong> এবং (২) <strong>message passing</strong>.</p>

<h2 style="text-align: justify;">Shared-Memory Systems</h2>
Shared Memory system মডেলে communicate করার ্জন্য একটি shared memory region এর প্রয়োজন।  সাধারণত, এই shared region প্রসেসের address স্পেসের পাশে তৈরী হয়ে থাকে। অন্য প্রসেসগুলো এই shared region এর সাথে communicate করার জন্য অবশ্যই তাদের address space এ এটাকে অ্যাটাচ/সংযুক্ত করার লাগবে। প্রসেসগুলো এই shared region এ ডাটা write / read এর মাধ্যমে communicate করে থাকবে। এই স্থানের ডাটা এবং লোকেশন অপারেটিং সিস্টেমের নিয়ন্ত্রনে থাকবে না, সম্পূর্ণ প্রসেসগুলোর নিয়ন্ত্রনে থাকবে।
<h2>Message-Passing Systems</h2>
<p style="text-align: justify;">Message Passing system এ প্রসেসগুলো তাদের মধ্যে একই address space ব্যবহার না করে শুধুমাত্র message pass করার মধ্যে কমিউনিকেট করে থাকে। Distributed environment এর ক্ষেত্রে এই ধরনের সিস্টেম কার্যকর। যেমন চ্যাটিং অ্যাপলিকেশনে একটি পিসি থেকে মেসেজ সেন্ড করা হয় এবং অন্য একটি পিসি থেকে সেই মেসেজ রিসিভ করা হয়। Message Passing System এ অবশ্যই send(message) এবং receive(message) এই দুটি ফিচার থাকতে হবে।</p>

<h3 style="text-align: justify;">⇒Naming</h3>
প্রসেসগুলো যখন নিজেদের মধ্যে কমিউনিকেট করবে তখন অবশ্যই তাদেরকে রেফার করা জন্য Name থাকবে। তারা হয় direct না হয় indirect communication করতে পারে।

Direct communication এর ক্ষেত্রে সেন্ডার এবং রিসিভার এর নাম অবশ্যই  কমিউনিকেট করার সময় পাস করতে হবে। এই স্কিমা অনুসারে send() এবং receive() ফাংশন হবেঃ
<ul>
	<li>send(P, message) - Send a message to process P</li>
	<li>receive(Q, message) - Receive a message from process Q</li>
</ul>
<h3 style="text-align: justify;">⇒Synchronization</h3>
ম্যাসেজ পাসিং সাধারণত send() এবং receive() ফাংশনের মাধ্যমে হয়ে থাকে। এছাড়াও এটি অন্যভাবেও ডিজাইন করো যায়। Message Passing টা blocking ( synchronous) অথবা nonblocking (asynchronous) হিসেবেও পাস হতে পারে।
<ul>
	<li><strong>Blocking Send:</strong> এ পদ্ধতিতে Receiving process যতক্ষন না পর্যন্ত মেসেজ রিসিভ করে ততক্ষন পর্যন্ত Sending Process ব্লক অবস্থায় অপেক্ষা করতে হয়।</li>
	<li><strong>Nonblocking Send:</strong> এ পদ্ধতিতে sending process মেসেজ সেন্ড করে এবং তার অপারেশন আবার শুরু করে।</li>
	<li><strong>Blocking Receive:</strong> মেসেজ অ্যাভইলেবল না হওয়া পর্যন্ত receiver blocked অবস্থায় থাকে।</li>
	<li><strong>Nonblocking Receive:</strong> Receiver হয় কোন বৈধ মেসেজ রিসিভ করে অথবা NULL মেসেজ রিসিভ করে।</li>
</ul>
<h3 style="text-align: justify;">⇒Buffering</h3>
মেসেজ এক্সেঞ্জের সময় একটি টেম্পরারি স্টোরেজের প্রয়োজন হয় ( Temporary Queue ) একেই সাধারণত বাফার বলে। এরকম queue ৩ ভাবে তৈরী করা যেতে পারেঃ
<ul>
	<li><strong>Zero Capacity:</strong> এই কিউর সর্বোচ্চ ধারনক্ষমতা হবে ০ ! অর্থাৎ কোনো মেসেজ এর মধ্যে অপেক্ষা করতে পাবে না। এমতাবস্থায় সেন্ডারকে অবশ্যই ব্লক থাকতে হবে রিসিভার মেসেজ রিসিভ না করা পর্যন্ত !</li>
	<li><strong>Bounded Capacity:</strong> এই কিউর কিছু নির্দিষ্ট ধারনক্ষমতা n থাকবে। অর্থাৎ এর মধ্যে সর্বোচ্চ n সংখ্যক মেসেজ থাকতে পারবে। মেসেজ পূর্ণ হলে sender ব্লক হবে।</li>
	<li><strong>Unbounded Capacity:</strong> এই কিউর দৈর্ঘ্য অসীম! এতে যেকোনো সংখ্যক মেসেজ রাখা সম্ভব। এবং সেন্ডার কখনও ব্লক হবে না !</li>
</ul>
&nbsp;
