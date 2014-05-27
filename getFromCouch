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

public class getFromCouch {

	public static void main(String args[]) {
		CouchDbClient dbClient1 = new CouchDbClient(args[0]);
		String csvFile1Path = args[1]+"_Positive"+".csv";  
		String csvFile2Path = args[1]+"_Negetive"+".csv"; 
		CsvWriter wr1 =new CsvWriter(csvFile1Path.replace('/', '_'),',',Charset.forName("SJIS"));  
		CsvWriter wr2 =new CsvWriter(csvFile2Path.replace('/', '_'),',',Charset.forName("SJIS")); 
		String csv[]=new String[4];
		int p=0;
		int n=0;
		int N=0;
		int Sen=0;
		
		List<JsonElement> list = dbClient1.view(args[1])
				.groupLevel(0).query(JsonElement.class);
		// System.err.println(list.get(0).toString());

		for (int i = 0; i < list.size(); i++) {
			for (Entry<String, JsonElement> entry : list.get(i)
					.getAsJsonObject().entrySet()) {
				if (entry.getKey().equals("key")) {
					// System.err.println(entry.getValue().toString());
					String tmp = entry.getValue().toString();

					JsonElement tweet = dbClient1.find(JsonElement.class,
							tmp.substring(1, tmp.length() - 1));

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
					
					
					if(Sen==0)
					{
						N++;
						
					}
					if(Sen>0)
					{
						p++;
					/*	try {
							wr1.writeRecord(csv);
						} catch (IOException e) {
							// TODO Auto-generated catch block
							e.printStackTrace();
						}*/
					}
					if(Sen<0)
					{
						n++;
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
			fw.write(args[1]+":Positive "+p+" Negetive "+n+" Neutral "+N+"\n");
			fw.close();
			wr1.close();
			wr2.close();
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
