API for Counting URLs in the WebPage



package url;

import java.net.URL;

import java.io.InputStreamReader;
import java.io.Reader;

import java.net.URI;

import java.net.URLConnection;

import javax.swing.text.EditorKit;
import javax.swing.text.SimpleAttributeSet;
import javax.swing.text.html.HTML;
import javax.swing.text.html.HTMLDocument;
import javax.swing.text.html.HTMLEditorKit;

public class CountingURLs {

	public void CountURL() throws Exception {
		int count=0;
		
		URL url = new URI("http://www.google.co.in").toURL();
		URLConnection conn = url.openConnection();
		Reader rd = new InputStreamReader(conn.getInputStream());

		EditorKit kit = new HTMLEditorKit();
		HTMLDocument doc = (HTMLDocument) kit.createDefaultDocument();
		kit.read(rd, doc, 0);

		HTMLDocument.Iterator it = doc.getIterator(HTML.Tag.A);
		while (it.isValid()) {
			if(it.isValid()){
			SimpleAttributeSet s = (SimpleAttributeSet) it.getAttributes();

			String link = (String) s.getAttribute(HTML.Attribute.HREF);
			if (link != null) {
				System.out.println(link);
			}
			count++;
			it.next();
			
			}
			
		}
		System.out.println(count);
	}
}



package url;

public class URLCounter {

	
	public static void main(String[] args) throws Exception {
		CountingURLs countingUrl=new CountingURLs();
		countingUrl.CountURL();

	}

}


API Reading XML file in java


import java.io.File;
import org.w3c.dom.*;

import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import org.xml.sax.SAXException;
import org.xml.sax.SAXParseException;

public class ReadAndWriteXMLFile {

	public void readFile() {
		try {

			DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory
					.newInstance();
			DocumentBuilder docBuilder = docBuilderFactory.newDocumentBuilder();
			Document doc = docBuilder.parse(new File(
					"G:/CoreJava/Programs/XML/Employee.xml"));

			// normalize text representation
			doc.getDocumentElement().normalize();

			NodeList listOfEmployees = doc.getElementsByTagName("employee");

			for (int s = 0; s < listOfEmployees.getLength(); s++) {

				Node firstEmployeeNode = listOfEmployees.item(s);
				if (firstEmployeeNode.getNodeType() == Node.ELEMENT_NODE) {

					Element firstEmployeeElement = (Element) firstEmployeeNode;

					// -------
					NodeList firstIDList = firstEmployeeElement
							.getElementsByTagName("id");
					Element firstIDElement = (Element) firstIDList.item(0);

					NodeList textIDList = firstIDElement.getChildNodes();
					System.out.println(((Node) textIDList.item(0))
							.getNodeValue().trim());

					// -------
					NodeList NameList = firstEmployeeElement
							.getElementsByTagName("name");
					Element NameElement = (Element) NameList.item(0);

					NodeList textNList = NameElement.getChildNodes();
					System.out.println(((Node) textNList.item(0))
							.getNodeValue().trim());

					// ----
					NodeList genderList = firstEmployeeElement
							.getElementsByTagName("Gender");
					Element genderElement = (Element) genderList.item(0);

					NodeList textgenderList = genderElement.getChildNodes();
					System.out.println(((Node) textgenderList.item(0))
							.getNodeValue().trim());
					System.out.println("");

					// ------

				}// end of if 

			}// end of for loop 

		} catch (SAXParseException err) {
			System.out.println("** Parsing error" + ", line "
					+ err.getLineNumber() + ", uri " + err.getSystemId());
			System.out.println(" " + err.getMessage());

		} catch (SAXException e) {
			Exception x = e.getException();
			((x == null) ? e : x).printStackTrace();

		} catch (Throwable t) {
			t.printStackTrace();
		}
		// System.exit (0);

	}

	
}




public class ReadXMLFile {

	
	public static void main(String[] args) {
		ReadAndWriteXMLFile readFile = new ReadAndWriteXMLFile();
		readFile.readFile();
	}

}




API for converting Text to PDF



import java.io.BufferedReader;
import java.io.DataInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.InputStreamReader;
import com.itextpdf.text.Document;
import com.itextpdf.text.Element;
import com.itextpdf.text.Font;
import com.itextpdf.text.Paragraph;
import com.itextpdf.text.pdf.PdfWriter;

	
public class ConverterPDF {
	

		public static PdfWriter writer;

		public  void ConverterTextToPDF() throws Exception {

			// TODO Auto-generated method stub
			File file = new File("G:/CoreJava/TextToPdf/src/texttopdf/Simple.txt");

			if (file.getName().endsWith(".txt")) {

				if (convertTextToPDF(file)) {
					System.out.println("Text file successfully converted to PDF");
				}
			}

		}

		public static boolean convertTextToPDF(File file) throws Exception {

			FileInputStream fis = null;
			DataInputStream in = null;
			InputStreamReader isr = null;
			BufferedReader br = null;

			try {

				Document pdfDoc = new Document();
				String output_file = file.getParent() + "\\" + "sample.pdf";
				setWriter(PdfWriter.getInstance(pdfDoc,
						new FileOutputStream(output_file)));
				pdfDoc.open();
				pdfDoc.setMarginMirroring(true);
				pdfDoc.setMargins(36, 72, 108, 180);
				pdfDoc.topMargin();
				Font myfont = new Font();
				Font bold_font = new Font();
				bold_font.setStyle(Font.BOLD);
				bold_font.setSize(10);
				myfont.setStyle(Font.NORMAL);
				myfont.setSize(10);
				pdfDoc.add(new Paragraph("\n"));

				if (file.exists()) {

					fis = new FileInputStream(file);
					in = new DataInputStream(fis);
					isr = new InputStreamReader(in);
					br = new BufferedReader(isr);
					String strLine;

					while ((strLine = br.readLine()) != null) {

						Paragraph para = new Paragraph(strLine + "\n", myfont);
						para.setAlignment(Element.ALIGN_JUSTIFIED);
						pdfDoc.add(para);
					}
				} else {

					System.out.println("no such file exists!");
					return false;
				}
				pdfDoc.close();
			}

			catch (Exception e) {
				System.out.println("Exception: " + e.getMessage());
			} finally {

				if (br != null) {
					br.close();
				}
				if (fis != null) {
					fis.close();
				}
				if (in != null) {
					in.close();
				}
				if (isr != null) {
					isr.close();
				}

			}

			return true;
		}

		private static void setWriter(PdfWriter instance) {
			// TODO Auto-generated method stub
			
		}

		
	}



public class Converter {
	public static void main(String[] args) throws Exception {
		ConverterPDF converterPdf=new ConverterPDF();
		converterPdf.ConverterTextToPDF();
	}

}


API for Converting Text to Speech



import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import com.sun.speech.freetts.Voice;   
import com.sun.speech.freetts.VoiceManager;   


public class SpeechManager{
	
	private static final String VOICENAME = "kevin16";  
	  
	 public void TextToSpeech() {  
	  Voice voice;  
	  
	  // Taking instance of voice from VoiceManager factory.  
	  VoiceManager voiceManager = VoiceManager.getInstance();  
	  voice = voiceManager.getVoice(VOICENAME);  
	 
	  // Allocating voice  
	  voice.allocate();  
	 
	  // word per minute  
	  voice.setRate(120);  
	  voice.setPitch(150);  
	  System.out.print("Enter your text: ");  
	  
	  // open up standard input  
	  BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
	  try {  
	   
		  // Ready to speak  
	   voice.speak(br.readLine());  
	  } catch (IOException e) {  
	   // TODO Auto-generated catch block  
	   e.printStackTrace();  
	  }  
	 }  
}



public class TextToSpeechConverter {

	
	public static void main(String[] args) {
		SpeechManager speechManager=new SpeechManager();
		speechManager.TextToSpeech();

	}

}


API for sending Mail

package mailmanager.magiccoders;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Properties;

import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class JavaMail {
	String password;
	static String to;
	static String host;

	public void sendMail() throws IOException {

		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

		System.out.println("Username");
		final String username = br.readLine();
		System.out.println("Enter Password ");
		password = br.readLine();
		System.out.println("Enter  ToAddress ");
		to = br.readLine();
		host = "smtp.gmail.com";
		System.out.println("Enter Subject ");
		String subject = br.readLine();
		System.out.println("Enter Body ");
		String body = br.readLine();
		Properties props = new Properties();
		props.put("mail.smtp.auth", "true");
		props.put("mail.smtp.starttls.enable", "true");
		props.put("mail.smtp.host", "smtp.gmail.com");
		props.put("mail.smtp.port", "587");

		Session session = Session.getInstance(props,
				new javax.mail.Authenticator() {
					protected PasswordAuthentication getPasswordAuthentication() {
						return new PasswordAuthentication(username, password);
					}
				});

		try {

			Message message = new MimeMessage(session);

			message.setFrom(new InternetAddress(username));
			message.setRecipients(Message.RecipientType.TO,
					InternetAddress.parse(to));
			message.setSubject(subject);
			message.setText(body);

			Transport.send(message);

			System.out.println("Your Message Sent");

		} catch (MessagingException e) {
			throw new RuntimeException(e);
		}
	}

	public static String getTo() {
		return to;
	}

	public static void setTo(String to) {
		JavaMailExample1.to = to;
	}

	public static String getHost() {
		return host;
	}

	public static void setHost(String host) {
		JavaMailExample1.host = host;
	}

}


package mailmanager.magiccoders;

import java.io.IOException;

public class MailManager {

	
	public static void main(String[] args) throws IOException {
		JavaMailExample1 javaMail = new JavaMailExample1();
		javaMail.sendMail();
	}

}


API for playing Wave File


import java.io.File;
	import java.io.IOException;
	 
	import javax.sound.sampled.AudioFormat;
	import javax.sound.sampled.AudioInputStream;
	import javax.sound.sampled.AudioSystem;
	import javax.sound.sampled.Clip;
	import javax.sound.sampled.DataLine;
	import javax.sound.sampled.LineEvent;
	import javax.sound.sampled.LineListener;
	import javax.sound.sampled.LineUnavailableException;
	import javax.sound.sampled.UnsupportedAudioFileException;
	 

			public class AudioPlayer implements LineListener {
		     
		    /**
		     * this flag indicates whether the playback completes or not.
		     */
		    boolean playCompleted;
		     
		   
		    void play(String audioFilePath) {
		        File audioFile = new File(audioFilePath);
		 
		        try {
		            AudioInputStream audioStream = AudioSystem.getAudioInputStream(audioFile);
		 
		            AudioFormat format = audioStream.getFormat();
		 
		            DataLine.Info info = new DataLine.Info(Clip.class, format);
		 
		            Clip audioClip = (Clip) AudioSystem.getLine(info);
		 
		            audioClip.addLineListener(this);
		 
		            audioClip.open(audioStream);
		             
		            audioClip.start();
		             
		            while (!playCompleted) {
		                // wait for the playback completes
		                try {
		                    Thread.sleep(1000);
		                } catch (InterruptedException ex) {
		                    ex.printStackTrace();
		                }
		            }
		             
		            audioClip.close();
		             
		        } catch (UnsupportedAudioFileException ex) {
		            System.out.println("The specified audio file is not supported.");
		            ex.printStackTrace();
		        } catch (LineUnavailableException ex) {
		            System.out.println("Audio line for playing back is unavailable.");
		            ex.printStackTrace();
		        } catch (IOException ex) {
		            System.out.println("Error playing the audio file.");
		            ex.printStackTrace();
		        }
		         
		    }
		     
		    
		    public void update(LineEvent event) {
		        LineEvent.Type type = event.getType();
		         
		        if (type == LineEvent.Type.START) {
		            System.out.println("Playback started.");
		             
		        } else if (type == LineEvent.Type.STOP) {
		            playCompleted = true;
		            System.out.println("Playback completed.");
		        }
		 
		    }

	}







public class AudioPlayer1 {

	
	public static void main(String[] args) {
		String audioFilePath = "C:/Users/naresh/Music/songs/akasham badhalaina.wav";
	     AudioPlayer player = new AudioPlayer();
	     player.play(audioFilePath);

	}

}



API for Converting color image to grayScale image

import java.awt.image.BufferedImage;

import java.io.*;

import javax.imageio.ImageIO;

public class GrayScale {
	 public void GreyScale(){
		    BufferedImage img = null;
		    File f = null;

		    //read image
		    try{
		      f = new File("G:/new folder/notes/notes/friends/Camera Roll/picture260.jpg");
		      img = ImageIO.read(f);
		    }catch(IOException e){
		      System.out.println(e);
		    }

		    //get image width and height
		    int width = img.getWidth();
		    int height = img.getHeight();

		    //convert to grayscale
		    for(int y = 0; y < height; y++){
		      for(int x = 0; x < width; x++){
		        int p = img.getRGB(x,y);

		        int a = (p>>24)&0xff;
		        int r = (p>>16)&0xff;
		        int g = (p>>8)&0xff;
		        int b = p&0xff;

		        //calculate average
		        int avg = (r+g+b)/3;

		        //replace RGB value with avg
		        p = (a<<24) | (avg<<16) | (avg<<8) | avg;

		        img.setRGB(x, y, p);
		      }
		    }

		    //write image
		    try{
		      f = new File("C:/Users/naresh/Pictures/roses/rose1.jpg");
		      ImageIO.write(img, "jpg", f);
		      System.out.println("Image converted to GrayScale successfully");
		    }catch(IOException e){
		      System.out.println(e);
		    }
		  }
		}




public class ImageProceesor {

	 static public void main(String args[]) throws Exception 
	   {
	      GrayScale obj = new GrayScale();
	      obj.GreyScale();
	   }
}










