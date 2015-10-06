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




