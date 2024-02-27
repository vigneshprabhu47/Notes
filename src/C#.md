##### 1. Routine to get the full stacktrace of an Exception
```CS
using System;
using System.Text;
using System.Text.RegularExpressions;

namespace ANamespace
{
    public class AClass
    {
        const string E = "";

        /// <summary>
        ///     Compiles the error messages &amp; stacktraces of
        ///     the <see cref="Exception"/> object and the
        ///     inner exceptions recursively until the child
        ///     exception object is null.
        /// </summary>
        /// <param name="ex">Exception object.</param>
        /// <param name="currentDepth">
        ///     Recursion depth, starts at 1.
        /// </param>
        /// <returns>
        ///      A string containing all the error messages &amp; traces in this format:
        ///      <br />
        ///      <code>
        ///          [MESSAGE:&lt;message&gt;&#59;TRACE:&lt;stacktrace&gt;]
        ///          [MESSAGE:&lt;child exception message&gt;&#59;TRACE:&lt;child exception stacktrace&gt;]
        ///          .
        ///          .
        ///          .
        ///          [MESSAGE:&lt;inner most exception message&gt;&#59;TRACE:&lt;inner most exception stacktrace&gt;]
        ///      </code>
        /// </returns>
        public static string GetFullTraceFromException(Exception ex, int currentDepth = 1)
        {
            string trace = $"[MESSAGE:{ex?.Message ?? E};TRACE:{ex?.StackTrace ?? E}]";

            if (ex.InnerException != null)
            {
                trace +=
                    GetFullTraceFromException(
                        ex.InnerException,
                        currentDepth + 1);
            }

            // Replace all the line breaks, tabs & multiple
            // whitespaces with a single whitespace:
            if (currentDepth == 1)
            {
                trace =
                    Regex.Replace(
                        trace,
                        @"\s+|\t+|(\r?\n)+",
                        " ");
            }

            return trace;
        }
    }
}
```
