---
layout: post
title:  "Invoking Executables from PHP, Safely"
date:   2017-06-08 00:51:15 +0530
categories: stories
tags: [gsoc, librecores]
comments: true
redirect_to: 'https://medium.com/@agathver/invoking-executables-from-php-safely-d4e4cabe650f'
---

PHP is a versatile language, but often we face the need to execute an external program to accomplish a task, not easily done using PHP.

To execute external commands, PHP provides with few built-in functions: `exec`, `shell_exec`, `system` and `proc_open`. However, the former three, though easy to use, are vulnereble to shell injection, much like SQL injection, especially if the command contains some user input. To guard against shell injection, PHP provides with `escapeshellcmd` and `escapeshellargs`, but they are grossly inadequate in functionality, do not perform proper escaping for shells other than `bash` and are ridden with CVEs.

An example of shell injection
```php
// a naive whois service, do not use in real code, ever!
// whois.php
// ...
<?php
$domain = $_GET['domain'];
$output = shell_exec("whois $domain");
?>
// html goes here
<p><?= $output ?></p>
```

And if I sent a request like

```
// $domain = 'example.com && echo "malicious code"'
GET /whois.php?domain=example.com%20%26%26%20echo%20%22malicious%20code%22
```
The output would be:

```
//... whois result
malicious code
```
Pretty dangerous isn't ?

## Executing external executables, safely

### Solution 1: Use PHP inbuilt `escapeshellcmd()` / `escapeshellargs()`
Modifying the PHP script a little.

```php
// a naive whois service, safer, but still dont use it
// whois.php
// ...
<?php
$domain = $_GET['domain'];
$output = shell_exec(excapeshellcmd("whois $domain"));
?>
// html goes here
<p><?= $output ?></p>
```

But it had some few critical CVEs associated, and [some inherent flaws][1], such as:
- [CVE-2015-4642][2]
- [CVE-2016-10045][3]
- [CVE-2016-10074][4]

The later 2 are CVEs that impacted several popular PHP frameworks and CMSs used on the internet, directly due to the consequence of `escapeshellcmd()`, which means a significant portion of the internet was vulnereble.

Definitely, **not recommended**

### Solution 2: Classic UNIX fork/exec using `pnctl_fork()` and `pnctl_exec()`

The two functions are the part of PHP pnctl extension (POSIX Extensions). They behave like the normal UNIX fork() and exec() functions. They do not go through a shell, and thus are immune to shell injection attacks.

```php
// a naive whois service, safer, but still dont use it
// whois.php
// ...
// html goes here
<p>
<?php
$domain = $_GET['domain'];
$pid = pnctl_fork();

switch($pid) {
    case -1:
        http_response_code(500);
        echo 'Unable to process request';
    case 0: // we are the child
        exit(pntcl_exec('/usr/bin/whois', [$domain]));
    case 1:
        // all output by child is directed back to parent's stdout, as they share the same FD
        pnctl_waitpid($pid);
}
</p>
?>
```

While this is a secure and good way to execute processes, it has some drawbacks:
- You have no straightforward way to get any output from the executed command. While, you can output to a file or socket and retrieve data, it's bit cumbersome.
- It depends on the `pnctl` PECL extension and is not available out-of-the-box and most webhosts refuse to enable it.
- This does not work on non POSIX compilant OSes, as it depends on the availability on `fork` syscall, only present in POSIX compilant OSes.
- If not properly coded, the fork/exec may lead to [fork bombs] which may deplete system resources.

### Solution 3: PHP `proc_open`

This is by far the most flexible inbuilt function for executing executables and scripts. It also provides means to control various streams and file descriptors of the child.

Let's reimplement our whois script using `proc_open`

```php
//...
<p>
<?php
$domain = $_GET['domain'];

// some validation on the domain parameter is required, the attacker may pass custom options to the
// executable. However we assume that, our version of `whois` is secure enough to perfrom its own
// validation and discard illegal inputs
$cmd = 'whois '. $domain;

$descriptorspec = array(
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w") // stderr is a pipe that the child will write to
);

$cwd = '/tmp';
$pipes = [];

$process = proc_open($cmp, $descriptorspec, $pipes, $cwd);

if (is_resource($process)) {
    $output = stream_get_contents($pipes[0]);
    $error = stream_get_contents($pipes[1]);

    fclose($pipes[0]);
    fclose($pipes[1]);

    if(!proc_close($process)) {
        echo 'Error: ' . $error;
    } else {
        echo $output;
    }
}
?>
</p>
```

It is clearly more flexible, but care must be taken in windows to pass the option to bypass the shell (as it uses cmd to start the process).

However, it is cumbersome and involves a great deal of low-level access to a process.

### Solution 4: The Symfony Process component

> "[Symfony][5] is a set of reusable PHP components and a PHP framework for web projects."

The [Symfony Process][6] component provides a high level API to execute commands in sub-processes. Even though you may be using a different framework or no framework at all (seriously, you should), you can easily use this component through composer (`symfony/process`).

Apart from usual convenient methods for retrieving and streaming outputs from processes, it provides an object oriented access to processes and wraps unhelpful errors with exceptions. Also, it is even more flexible than the `proc_open`. (Internally, it builds on the same, with secure defaults)

Here is the code:

```php
<p>
<?php
use Symfony\Component\Process\Exception\ProcessFailedException;
use Symfony\Component\Process\Process;

$domain = $_GET['domain'];  // yes, there are more nicer ways to do this, but, it's a demo after all ;)
$process = new Process('whois '. $domain);  // do filter $domain

try {
    $process->mustRun();
    echo $process->getOutput();
} catch (ProcessFailedException $e) {
    echo $e->getMessage();
}
?>
</p>
```

Simple. It also has more goodies like timeout, streaming the output line by line.

## Background

In my [GSoC project][7] for [LibreCores.org][8], I had to invoke few `git` processses with known inputs, to extract info about commits and contributors.

In coming days, I will write about message queues and other interesting components that I am busy building these days.

## Resources

- **PHP Manual**
    * [`shell_exec`][9]
    * [`escapeshellcmd`][10]
    * [`escapeshellarg`][11]
    * [`pnctl_fork`][12]
    * [`pnctl_exec`][13]
    * [`pnctl_waitpid`][14]
    * [`proc_open`][15]
    * [`proc_close`][16]

- **Symfony documentation**
    * [Symfony Process component][17]

- **Other resources**
    * [Command Injection on OWASP][18]
    * [Discussion of shell injection in PHP][1]




[1]:https://gist.github.com/Zenexer/40d02da5e07f151adeaeeaa11af9ab36
[2]:http://www.cvedetails.com/cve/CVE-2015-4642/
[3]:https://legalhackers.com/advisories/PHPMailer-Exploit-Remote-Code-Exec-CVE-2016-10045-Vuln-Patch-Bypass.html
[4]:https://legalhackers.com/advisories/SwiftMailer-Exploit-Remote-Code-Exec-CVE-2016-10074-Vuln.html
[5]:https://symfony.com
[6]:http://symfony.com/doc/current/components/process.html
[7]:https://amitosh.in/stories/2017/05/23/summer-code-and-librecores/
[8]:https://www.librecores.org
[9]:http://php.net/manual/en/function.shell-exec.php
[10]:http://php.net/manual/en/function.escapeshellcmd.php
[11]:http://php.net/manual/en/function.escapeshellarg.php
[12]:http://php.net/manual/en/function.pcntl-fork.php
[13]:http://php.net/manual/en/function.pcntl-exec.php
[14]:http://php.net/manual/en/function.pcntl-waitpid.php
[15]:http://php.net/manual/en/function.proc-open.php
[16]:http://php.net/manual/en/function.proc-close.php
[17]:http://symfony.com/doc/current/components/process.html
[18]:https://www.owasp.org/index.php/Command_Injection
