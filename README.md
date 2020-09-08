# Lab2
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.Arrays;
import java.util.Scanner;

public class SongsReport {

   public static void main(String[] args) {
      
       //loading name of file
       File file = new File("songs.csv"); //reading data from this file
       //scanner to read java file
       Scanner reader;
       //line to get current line from the file
       String line="";

       //length of possible maximum length of records in the file
       int maxArtistPossible = 1000;

       //initialize array with the help of maxArtistPossible
       String artists[] = new String[maxArtistPossible];
       //another array to store the count for each artist
       int artistsCount[] = new int[maxArtistPossible];

       //it will store the actual number of records read from the file
       int currentIndex=0;
       //reset count to zero
       for(int i=0;i<artistsCount.length;i++){
           artistsCount[i] = 0;
       }
       //store total number of songs
       int songRecordCount=0;

       //try block to read the file
       try {
           //start file reader
           reader = new Scanner(file);
           //check if reader has more lines to read
           while(reader.hasNextLine()){
               //increase the song count by one
               songRecordCount++;
               //read line
               line = reader.nextLine();
               //split current line by character
               String columns[] = line.split(",(?=(?:[^\"]*\"[^\"]*\")*[^\"]*$)");

               //after split artist name will be at possition 2
               //Artist will be at position 2
               String tmpArtist = columns[2];

               //remove quotes from the line
               tmpArtist = tmpArtist.replaceAll("\"", "");
               //now artist list also can have many artist in it. So split that by comma
               for(String art : tmpArtist.split(",")){
                   boolean found = false;
                   //loop and check if current artist already in list or not
                   for(int i=0;i<currentIndex;i++){
                       //if artist found in the list
                       if(art.equalsIgnoreCase(artists[i])){
                           //increase it's count by one
                           artistsCount[i]++;
                           found = true;
                           break;
                       }
                   }
                   //if artist not found in the list
                   if(!found){
                       //make new entry in the list
                       artists[currentIndex] = art;
                       artistsCount[currentIndex]=1;
                       currentIndex++;
                   }
               }
           }
       } catch (FileNotFoundException e) {
           System.out.println("file not found");
       }

       //print data
       System.out.println("Total song loaded : "+songRecordCount);
       System.out.println("Total unique artist count is : "+currentIndex);
       System.out.printf("%-25s%s\n","Artist","Occurence count");
       for(int i=0;i<currentIndex;i++){
           System.out.printf("%-25s%s\n",artists[i],artistsCount[i]);
       }
   }


}
