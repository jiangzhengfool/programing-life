## Udp.java
~~~
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;
import java.net.UnknownHostException;

/**
 * 
 * Project name：codeSnippet     
 *  
 * Class decription：  UDP编程的例子
 * Class name：me.wwfeng.javase.NetworkProgramming.Udp       
 * Author：wwfeng  
 * Create date：2017年4月6日 下午11:27:53
 */
public class Udp {

}

class Dgram {

	public static DatagramPacket toDatagram(String s, InetAddress destIA, int destPort) {
		byte[] buf = new byte[s.length() + 1];
		s.getBytes(0, s.length(), buf, 0);
		return new DatagramPacket(buf, buf.length, destIA, destPort);
	}

	public static String toString(DatagramPacket p) {
		return new String(p.getData(), 0, p.getLength());
	}
}

class ChatterServer {
	static final int INPORT = 1712;
	private byte[] buf = new byte[1000];
	private DatagramPacket dp = new DatagramPacket(buf, buf.length);
	private DatagramSocket socket;

	public ChatterServer() {
		try {
			socket = new DatagramSocket(INPORT);// 创建一接收消息的对象，而不是每次接收消息都创建一个
			System.out.println("Server started");
			while (true) {
				socket.receive(dp);
				// 接收到客户端的消息
				String rcvd = Dgram.toString(dp) + ",from address:" + dp.getAddress() + ",port:" + dp.getPort();
				System.out.println("From Client:" + rcvd);

				String echoString = "From Server Echoed:" + rcvd;
				DatagramPacket echo = Dgram.toDatagram(echoString, dp.getAddress(), dp.getPort());
				// 将数据包发送给客户端
				socket.send(echo);
			}
		} catch (SocketException e) {
			System.err.println("Can't open socket");
			System.exit(1);
		} catch (IOException e) {
			System.err.println("Communication error");
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		new ChatterServer();
	}
}

class ChatterClient extends Thread {

	private DatagramSocket s;
	private InetAddress hostAddress;
	private byte[] buf = new byte[1000];
	private DatagramPacket dp = new DatagramPacket(buf, buf.length);
	private int id;

	public ChatterClient(int identifier) {
		id = identifier;
		try {
			s = new DatagramSocket();
			hostAddress = InetAddress.getByName("localhost");

		} catch (UnknownHostException e) {
			System.err.println("Cannot find host");
			System.exit(1);
		} catch (SocketException e) {
			System.err.println("Can't open socket");
			e.printStackTrace();
			System.exit(1);
		}
		System.out.println("ChatterClient starting");
	}

	public void run() {
		try {
			for (int i = 0; i < 25; i++) {
				String outMessage = "Client #" + id + ",message #" + i;
				s.send(Dgram.toDatagram(outMessage, hostAddress, ChatterServer.INPORT));
				s.receive(dp);
				String rcvd = "Client #" + id + ",rcvd from " + dp.getAddress() + ", " + dp.getPort() + ":"
						+ Dgram.toString(dp);
				System.out.println(rcvd);
			}
		} catch (IOException e) {
			e.printStackTrace();
			System.exit(1);
		}
	}

	public static void main(String[] args) {
		for (int i = 0; i < 10; i++) {
			new ChatterClient(i).start();
		}
	}
}
~~~