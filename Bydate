import java.io.BufferedReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.Charset;
import java.util.List;
import java.util.Map.Entry;
import java.util.Set;

import org.lightcouch.CouchDbClient;

import com.csvreader.CsvWriter;
import com.google.gson.JsonElement;

public class Bydate {

	public static void main(String args[]) {
		CouchDbClient dbClient1 = new CouchDbClient(args[0]);
	//	String csvFile1Path = args[1]+"_Positive"+".csv";  
		//String csvFile2Path = args[1]+"_Negetive"+".csv"; 
	//	CsvWriter wr1 =new CsvWriter(csvFile1Path.replace('/', '_'),',',Charset.forName("SJIS"));  
		//CsvWriter wr2 =new CsvWriter(csvFile2Path.replace('/', '_'),',',Charset.forName("SJIS")); 
		//String csv[]=new String[4];
		int p=0;
		int n=0;
		int N=0;
		int Sen=0;
		int result [][]=new int [31][3];
		List<JsonElement> list = dbClient1.view(args[1])
				.groupLevel(2).query(JsonElement.class);
		// System.err.println(list.get(0).toString());

		for (int i = 0; i < list.size(); i++) {
			for (Entry<String, JsonElement> entry : list.get(i)
					.getAsJsonObject().entrySet()) {
				//System.err.println(entry.getKey());
				if (entry.getKey().equals("key")) {
					// System.err.println(entry.getValue().toString());
					String raw = entry.getValue().toString();
					//System.err.println(raw);
					String [] tmp=raw.split(",");
					JsonElement tweet = dbClient1.find(JsonElement.class,
							tmp[1].substring(1, tmp[1].length() - 2));

					for (Entry<String, JsonElement> rawTweet : tweet
							.getAsJsonObject().entrySet()) {
						if(rawTweet.getKey().equals("geoLocation"))
						{
							//csv[0]=rawTweet.getValue().toString().substring(12, 32);
							//csv[1]=rawTweet.getValue().toString().substring(47, 57);
						}
						if (rawTweet.getKey().equals("text")) {
							Sen=SentimentAnalysis.findSentiment(rawTweet.getValue()
											.toString());
							
							//csv[2]=rawTweet.getValue().toString();
							//csv[3]=String.valueOf(Sen);
							//System.err.println(rawTweet.getValue().toString());
						}
					
					}
					//System.err.println(tmp[0]);
					int index=Integer.valueOf(tmp[0].substring(1));
					
					if(Sen==0)
					{
						result[index][1]++;
						
					}
					if(Sen>0)
					{
						result[Integer.valueOf(index)][2]++;
					/*	try {
							wr1.writeRecord(csv);
						} catch (IOException e) {
							// TODO Auto-generated catch block
							e.printStackTrace();
						}*/
					}
					if(Sen<0)
					{
						result[Integer.valueOf(index)][0]++;
					/*	try {
							wr2.writeRecord(csv);
						} catch (IOException e) {
							// TODO Auto-generated catch block
							e.printStackTrace();
						}*/
					}
					// BufferedReader in = new BufferedReader(new
					// InputStreamReader(in_db));
					// try {
					// System.err.println(in.readLine());
					// } catch (IOException e) {
					// TODO Auto-generated catch block
					// e.printStackTrace();
				}
			}
		}
		try {
			FileWriter fw=new FileWriter(("SentiAnalysis_"+args[1].replace('/', '_')+".txt"),true);
			fw.write(args[1]);
			for(int i=1;i<31;i++)
			{
				fw.write(i+" :Positive "+result[i][2]+" Negetive "+result[i][0]+" Neutral "+result[i][1]);
				fw.write("\n");
			}
			fw.close();
			//wr1.close();
			//wr2.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

	// ..
	// try {
	// in.close();
	// } catch (IOException e) {
	// TODO Auto-generated catch block
	// e.printStackTrace();
	// } // close stream
}
