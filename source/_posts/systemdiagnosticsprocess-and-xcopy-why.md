---
title: "System.Diagnostics.Process and xcopy... why it doesn't work?"
date: 2009-03-31 22:41:00
tags: [c#,deployment]
---

We spent 30 minutes on a spiny issue. We execute some commands directly from a C# program but we want to make sure that everything executed. We did unit test on some code and everything seemed to work.

Today, we tried it live with some real case scenario where we execute some XCOPY. And then.... nothing.

We didn't receive any ExitCode that was 0\. We didn't catch any exception. And we didn't catch anything in the error stream.

Here is the code that we used:

```cs
private void ExecuteProcess()
{
    // setup the process info
    var startInfo = new ProcessStartInfo(@"cmd.exe", @" /c xcopy c:\file*.txt c:\file*.bck")
        {
            UseShellExecute = false,
            CreateNoWindow = true,
            WorkingDirectory = @"C:\",
            RedirectStandardError = true,
            RedirectStandardOutput = true
        };

    using(Process proc = Process.Start(startInfo))
    {
        proc.OutputDataReceived += ((sender, e) => MessageBox.Show("Data: " + e.Data ?? String.Empty));
        proc.ErrorDataReceived +=
            (sender, e) => MessageBox.Show(string.Format("Error: {0}", e.Data ?? string.Empty));
        proc.BeginErrorReadLine();
        proc.BeginOutputReadLine();

        proc.WaitForExit();
    }

}
```

Wow. Nice piece of code isn't it? It will work for everything you have to execute. But if you execute XCOPY it won't work. We found out that it's missing something that should not be needed in our case but that cause XCOPY to just execute with no output and no results.

You want XCOPY to work? Just add this line before starting the process:

```cs
startInfo.RedirectStandardInput = true;
```

That's it! This error will only happen if you have `UseShellExecute` set to `false`.

Hope it help some clueless programmer that is wondering why the XCOPY won't execute.

