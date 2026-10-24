<h1>Containers from scratch</h1>
<p>In this project, I'll be building a program that creates a new container, similar to docker run.</p>

<p>I'll update this with my changes at each stage and goals for the following stages.</p>

<h2>Logging command</h2>
<p>First commit simply logs the command passed in:</p>
<img width="610" height="828" alt="image" src="https://github.com/user-attachments/assets/bfb2170c-9ae6-475a-b3a4-d58d6f4b6a60" />
<p>Passing echo hello world!:</p>
<img width="789" height="60" alt="image" src="https://github.com/user-attachments/assets/6c78d7d9-f067-42cb-b7b7-0a53c5ee5300" />

<p>Next I'd like to actually run the command that is passed in.</p>

<h2>Converting args to command and executing</h2>
<p>Args are converted to a command and executed. Stdin/out/err are wired up to output to the terminal.</p>
<img width="681" height="322" alt="image" src="https://github.com/user-attachments/assets/1464f9d8-b2aa-4459-9ff4-a7a987877b3c" />
<p>Passing echo hello world!:</p>
<img width="781" height="81" alt="image" src="https://github.com/user-attachments/assets/8ccd91e4-98fd-4e1f-87ff-8f9e25c02405" />
<p>The command is successfully executed. I can also run a shell:</p>
<img width="694" height="264" alt="image" src="https://github.com/user-attachments/assets/cffc4baa-7ac8-4b20-87de-bdcc0f653c1d" />
<p>It isn't clear that the terminal is using that shell, but we can see the shell running. I'd like to address this in the next commit by specifying a namespace for this command.</p>
