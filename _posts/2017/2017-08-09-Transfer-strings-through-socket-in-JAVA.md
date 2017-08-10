---
layout: post
title: Socket 类示例
category: java
tags: [java]
---

*As a beginner of java, I have encountered some problems when I used Socket in early days. They let me wasted a lot of time to solve. Today, I have free time, so, I organize the problems and solutions I met.*    

## class `PrintWriter`
class `PrintWriter` have two different method `write` and `println`. The same to write msg to stdout stream, msg will be write to stdout only when buffer fulled or encountered '\n', '\r', etc.    
`write` hava a kind of overload is `write(String s)`, when use it in code, '\n' or '\r' must be add in the back of `s`. If not, program is blocking in this.  
And the method `flush()` is important, too.
it can refresh buffer, and ensure all msg is in stream instead of any internal buffer.    

<br/>
## class `BufferedReader`
class `BufferedReader` have a very useful method `readLine()`, however, it's a blocking method.  
It means that 'readLine()' will block instead of read null when stream finish, program will pause in this place until stream insert new msg or close.

## Following is complete Code
**CLient Code:**
{% highlight java %}
import java.io.*;
import java.net.Socket;

/**
 * @author snowwhisper
 *
 * class {@code Client} is a simple socket client example.
 * It only could connect to the server, output and input a
 * string.
 */
public class Client {
    //Specify the server ip address and port
    private static String ipAddress = "127.0.0.1";
    private static int port = 2017;

    /**
     * main method of program
     * @param args command line arguments
     */
    public static void main(String[] args){
        Socket socket = null;

        OutputStream outputStream = null;
        PrintWriter printWriter = null;

        InputStream inputStream = null;
        BufferedReader bufferedReader = null;

        try {
            // init socket, connect to the server
            socket = new Socket(ipAddress, port);

            // get output stream of socket
            outputStream = socket.getOutputStream();
            printWriter = new PrintWriter(outputStream);

            // insert a string into the output stream
            // if use write method, '\n' or '\r' is necessary
            printWriter.println("Hello, I am client");
            /* ensure that anything written to the writer prior si written to
             * the underlying stream, rather than stay in some internal buffer*/
            printWriter.flush();

            // get input stream of socket
            inputStream = socket.getInputStream();
            bufferedReader = new BufferedReader(new InputStreamReader(inputStream));

            String info = null;
            //read msg from buffer, and storage it in info, and print info
            while ((info = bufferedReader.readLine()) != null)
                System.out.println(info);
        }
        //init socket connection will throw a IOException, must catch or throw it
        catch (IOException e) {
            e.printStackTrace();
        }
        //close connection is important and necessary
        finally {
            try {
                if(socket != null)
                    socket.close();
                if(outputStream != null)
                    outputStream.close();
                if(printWriter != null)
                    printWriter.close();
                if(inputStream != null)
                    inputStream.close();
                if(bufferedReader != null)
                    bufferedReader.close();
            } catch (IOException e){
                e.printStackTrace();
            }
        }

    }
}
{% endhighlight %}

**Server Code**
{% highlight java %}
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @author snowwhisper
 *
 * class {@code Server} is only used as program entry
 */
public class Server {
    public static void main(String[] args){
        ListenClient listenClient = new ListenClient(2017);
        listenClient.start();
    }
}

/**
 * A {@code ListenClient} object represents a listen port.
 */
class ListenClient{
    private ServerSocket serverSocket = null;
    private int port;
    private boolean ifContinueListen = true;

    /**
     * Class constructor
     * @param port the port is to listen
     */
    public ListenClient(int port){
        try {
            this.port = port;
            serverSocket = new ServerSocket(port, 3);
        }
        catch (IOException e){
            e.printStackTrace();
        }
    }

    /**
     * Start listen port
     */
    public void start(){
        Socket socket = null;
        ifContinueListen = true;

        while (ifContinueListen){
            try {
                socket = serverSocket.accept();

                //init a ServerThread object, and deal with socket
                //connection in a Independent thread
                ServerThread serverThread = new ServerThread(socket);
                serverThread.start();
            }
            catch (IOException e){
                e.printStackTrace();
            }
        }
    }

    /**
     * Stop listen port
     */
    public void stop(){
        ifContinueListen = false;
        try {
            //Establish a socket connection, end serverSocket.accept() blocking state
            Socket socket = new Socket("127.0.0.1", port);

            //close serverSocket, stop listen port
            serverSocket.close();
        }
        catch (IOException e){
            e.printStackTrace();
        }
    }
}

/**
 * A {@code ServerThread} object represents a thread, which deal with
 * a socket connection
 */
class ServerThread extends Thread{
    private Socket socket = null;

    /**
     * Class constructor
     * @param socket the connection from client
     */
    public ServerThread(Socket socket){
        this.socket = socket;
    }

    /**
     * extend from super class, the method execute when thread start
     */
    @Override
    public void run() {
        InputStream inputStream = null;
        BufferedReader bufferedReader = null;

        OutputStream outputStream = null;
        PrintWriter printWriter = null;

        try {
            //grammar the same to client
            inputStream = socket.getInputStream();
            bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
            String info = null;
            while ((info = bufferedReader.readLine()) != null) {
                System.out.println(info);
            }

            outputStream = socket.getOutputStream();
            printWriter = new PrintWriter(outputStream);
            printWriter.println("Hello, I am server");
            printWriter.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(socket != null)
                    socket.close();
                if(inputStream != null)
                    inputStream.close();
                if(bufferedReader != null)
                    bufferedReader.close();
                if(outputStream != null)
                    outputStream.close();
                if(printWriter != null)
                    printWriter.checkError();
            }
            catch (IOException e){
                e.printStackTrace();
            }
        }
    }
}
{% endhighlight %}
