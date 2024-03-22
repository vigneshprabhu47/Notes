##### 1. Routine to get the full stacktrace of an Exception object
```CS
using Newtonsoft.Json;
using Newtonsoft.Json.Serialization;

using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;

using static Newtonsoft.Json.JsonConvert;

namespace ANameSpace
{
    public class ErrorTrace
    {
        [JsonProperty("error")]
        public string Error { get; set; }

        [JsonProperty("trace")]
        public string Trace { get; set; }
    }

    public static class AClass
    {
        const string E = "";

        private static string GetSingleLineString(string s)
        {
            return Regex.Replace(
                s,
                @"\s+|\t+|(\r?\n)+",
                " ");
        }

        /// <summary>
        ///     Compiles the error messages &amp; stacktraces of
        ///     the <see cref="Exception"/> object and the
        ///     inner exceptions recursively.
        /// </summary>
        /// <param name="ex">Exception object.</param>
        /// <param name="multilineOutput">
        ///     If true, the linebreaks, tabs &amp; multiple
        ///     occurances of whitespaces will be replaced
        ///     by a single whitespace.
        /// </param>
        /// <returns>
        ///      A list of <see cref="ErrorTrace"/>.
        ///      <br />
        ///      Returns null on error.
        /// </returns>
        public static List<ErrorTrace> GetAllErrorTraces(Exception ex, bool multilineOutput = true)
        {
            try
            {
                var traces = new List<ErrorTrace>();
                var currentException = ex;

                if (multilineOutput)
                {
                    do
                    {
                        traces.Add(
                            new ErrorTrace
                            {
                                Error = currentException?.Message ?? E,
                                Trace = currentException?.StackTrace ?? E
                            });

                        currentException = currentException.InnerException;
                    } while (currentException.InnerException != null);
                }
                else
                {
                    do
                    {
                        traces.Add(
                            new ErrorTrace
                            {
                                Error = GetSingleLineString(currentException?.Message ?? E),
                                Trace = GetSingleLineString(currentException?.StackTrace ?? E)
                            });

                        currentException = currentException.InnerException;
                    } while (currentException.InnerException != null);
                }

                return traces;
            }
            catch
            {
                return null;
            }
        }

        /// <summary>
        ///     Compiles the error messages &amp; stacktraces of
        ///     the <see cref="Exception"/> object and the
        ///     inner exceptions recursively.
        /// </summary>
        /// <param name="ex">Exception object.</param>
        /// <param name="multilineOutput">
        ///     If true, the linebreaks, tabs &amp; multiple
        ///     occurances of whitespaces will be replaced
        ///     by a single whitespace.
        /// </param>
        /// <param name="maxLength">
        ///     Maximum length, default is zero.
        ///     <br />
        ///     If set to any value greater than zero,
        ///     a string with a maximum length as
        ///     specified will be returned.
        /// </param>
        /// <returns>
        ///      A string containing all the error messages &amp; traces in this format:
        ///      <br />
        ///      <code>
        ///          [
        ///              { error: &lt;message&gt;, trace: &lt;stacktrace&gt; },
        ///              { error: &lt;inner exception message&gt;, trace: &lt;inner exception stacktrace&gt; },
        ///              .
        ///              .
        ///              .
        ///              { error: &lt;inner most exception message&gt;, trace: &lt;inner most exception stacktrace&gt; }
        ///          ]
        ///      </code>
        ///      Returns null on error.
        /// </returns>
        public static string GetAllErrorTracesAsString(Exception ex, bool multilineOutput = true, int maxLength = 0)
        {
            try
            {
                var errs =
                    GetAllErrorTraces(ex, multilineOutput) ??
                    throw new Exception("No data");

                var res = SerializeObject(errs);

                // If output should to be truncated:
                return maxLength > 0 && maxLength < res.Length
                        ? res.Substring(0, maxLength)
                        : res;
            }
            catch
            {
                return null;
            }
        }
    }
}
```
