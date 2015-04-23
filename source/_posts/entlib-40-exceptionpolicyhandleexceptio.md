---
title: "EntLib 4.0 - ExceptionPolicy.HandleException is not thread safe"
date: 2009-05-05 21:26:00
tags: []
---

We've faced this problem recently where Enterprise Library was crashing. Not everywhere... just on this little line of code. We were trying to save to a database asynchronously but if the database was not available, it threw an exception which Enterprise Library was supposed to catch.

However, this never happened. Enterprise library crashed on us. Mind you, we didn't figure out this problem until recently because the problem was just happening in production.

When we managed to reproduce the exception in a development environment (where debugging is possible), we got an error similar to "An item with the same key has already been added". Here's the beginning of the stack trace:

> Microsoft.Practices.EnterpriseLibrary.ExceptionHandling.ExceptionHandlingException: The current build operation (build key Build Key[Microsoft.Practices.EnterpriseLibrary.ExceptionHandling.ExceptionPolicyImpl, LogException]) failed: The current build operation (build key Build Key[Microsoft.Practices.EnterpriseLibrary.Logging.LogWriter, null]) failed: An item with the same key has already been added.

That lead me to [this thread](http://www.codeplex.com/entlib/Thread/View.aspx?ThreadId=32286) where it was mentioned that an issue was logged. I found [this issue](http://www.codeplex.com/entlib/WorkItem/View.aspx?WorkItemId=17534) which mentioned concurrency issue. Since our code was multi-threaded, we had two options. Replace Enterprise Library 4.0 by Enterprise Library 4.1 or make a thread safe wrapper for the call to the log exception.

The first option required reconfiguring the build and all development machine, the second one consisted of coding a dirty patch. We went with the first one (what else did they fix in 4.1?) for obvious reason.

So, if you have this kind of problem with Enterprise Library 4.0, you might want to upgrade to Enterprise Library 4.1\. Since the problem happen in the error handling section, it makes this kind of bug hard to understand and erratic (due to concurrency).
