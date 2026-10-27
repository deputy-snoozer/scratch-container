<h1>Containers from scratch</h1>
<p>In this project, I'll be building a program that creates a new container, similar to docker run.</p>

<p>I'll update this with my changes at each stage and goals for the following stages.</p>

<h2>Logging command</h2>
<p>First commit simply logs the command passed in:</p>
<img width="610" height="828" alt="image" src="https://github.com/user-attachments/assets/bfb2170c-9ae6-475a-b3a4-d58d6f4b6a60" />
<p>Passing <code>echo hello world!</code>:</p>
<img width="789" height="60" alt="image" src="https://github.com/user-attachments/assets/6c78d7d9-f067-42cb-b7b7-0a53c5ee5300" />

<p>Next I'd like to actually run the command that is passed in.</p>

<h2>Converting args to command and executing</h2>
<p>Args are converted to a command and executed. Stdin/out/err are wired up to output to the terminal.</p>
<img width="681" height="322" alt="image" src="https://github.com/user-attachments/assets/1464f9d8-b2aa-4459-9ff4-a7a987877b3c" />
<p>Passing <code>echo hello world!</code>:</p>
<img width="781" height="81" alt="image" src="https://github.com/user-attachments/assets/8ccd91e4-98fd-4e1f-87ff-8f9e25c02405" />
<p>The command is successfully executed. I can also run a shell:</p>
<img width="694" height="264" alt="image" src="https://github.com/user-attachments/assets/cffc4baa-7ac8-4b20-87de-bdcc0f653c1d" />
<p>It isn't clear that the terminal is using that shell, but we can see the shell running. I'd like to address this in the next commit by specifying a namespace for this command.</p>

<h2>Setting namespace for process</h2>
<p>The process now invokes itself inside a new UTS namespace. It uses <code>/proc/self/exe</code> to reference its own executable, passing the <code>child</code> command along with the original args.</p>
<img width="1084" height="435" alt="image" src="https://github.com/user-attachments/assets/60175ca6-4250-4e17-90d6-18c1dcc7651a" />
<p>The child process sets the hostname to <code>container</code>:</p>
<img width="658" height="370" alt="image" src="https://github.com/user-attachments/assets/87be2a56-63ae-4ca0-a428-8e22c5019e99" />
<p>Now the shell is clearly running in the new namespace:</p>
<img width="981" height="160" alt="image" src="https://github.com/user-attachments/assets/2b633cb7-586d-45d1-a8d1-b2394ebfedb1" />
<p>This process can still see processes running on the host machine:</p>
<img width="669" height="208" alt="image" src="https://github.com/user-attachments/assets/a4288014-efb4-4c08-8c24-217e5ae2b578" />
<p>I plan to address this in the next commit.</p>

<h2>Adding a PID namespace and chroot</h2>
<p>The child process now starts in a new PID namespace and changes its root directory to a prepared Ubuntu filesystem at <code>/home/alex/vagrant/ubuntu-fs</code>. This lets it run commands, such as Bash, from inside that filesystem.</p>
<img width="985" height="106" alt="image" src="https://github.com/user-attachments/assets/4d3b8971-e266-4c86-8f77-d4707827f20c" />
<img width="1359" height="90" alt="image" src="https://github.com/user-attachments/assets/2eb6e3b9-dd7d-43fe-be29-3653e2233cb4" />
<p>The filesystem path is specific to my WSL setup and needs to be changed if the root filesystem is stored somewhere else.</p>
<p>Running a sleep command on the container and then checking sleep processes on the host:</p>
<img width="306" height="19" alt="image" src="https://github.com/user-attachments/assets/9698bc94-0a56-481a-a95c-038293114db9" />
<p>I can see all the information stored for that process in the dedicated <code>/proc/<PID></code> directory:</p>
<img width="1311" height="264" alt="image" src="https://github.com/user-attachments/assets/3e2c3831-b9c6-4a53-9e3f-a245d4fdda96" />
<p>Including the root directory of the process:</p>
<img width="951" height="70" alt="image" src="https://github.com/user-attachments/assets/76edf3d5-1170-47d3-b215-9b36ecbe1b80" />
<p>Processes still need a separate <code>/proc</code> mount to be visible to tools like <code>ps</code> inside the PID namespace; I plan to address this next.</p>
<img width="442" height="49" alt="image" src="https://github.com/user-attachments/assets/4081f19d-c2f8-4a56-8e26-04348432fd89" />
