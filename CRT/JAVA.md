


Access Modifier:
Access modifiers are keywords in object-oriented languages that set the accessibility of classes, methods, and other members. Access modifiers are a specific part of programming language syntax used to facilitate the encapsulation of components.

Public
Private
Protected


``` java

import java.util.*;
public class Main
{
	public static void main(String[] args) {
		Scanner sc=new Scanner(System.in);
		int round=sc.nextInt();
		sc.nextLine();
		String str=sc.nextLine();
		System.out.print(game(round,str));
	}
	static int game(int round,String str)
	{
	    int p1Score=0;
	    int idx=0;
	    for(int i=0;i<round;i++)
	    {
	        String p1Move="";
	        if(str.startsWith("snake",idx))
	        {
	            p1Move="snake";
	            idx+=5;
	        }
	        else if(str.startsWith("water",idx))
	        {
	            p1Move="water";
	            idx+=5;
	        }
	        else  if(str.startsWith("gun",idx))
	        {
	            p1Move="gun";
	            idx+=3;
	        }
	        
	         String p2Move="";
	        if(str.startsWith("snake",idx))
	        {
	            p2Move="snake";
	            idx+=5;
	        }
	        else if(str.startsWith("water",idx))
	        {
	            p2Move="water";
	            idx+=5;
	        }
	        else  if(str.startsWith("gun",idx))
	        {
	            p2Move="gun";
	            idx+=3;
	        }
	        if(p1Move.equals("snake") && p2Move.equals("water"))
	           p1Score++;
	        else if(p1Move.equals("water") && p2Move.equals("gun"))
	           p1Score++;
	       else if(p1Move.equals("gun") && p2Move.equals("snake"))
	           p1Score++;        
	         
	    }
	    return p1Score;
	}
}
```
