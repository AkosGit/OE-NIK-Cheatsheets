# Java Cheatsheet
Intelij: alt+insert menu
## Types
- string to int
	- Integer.parseint("");
-  
# Files
- Writing
```java
 PrintWriter pw=new PrintWriter(new File("test.txt"));
 pw.println("Hello");
 pw.println("Töhötöm");
//Close PrintWriter
 pw.close();
 //Write lines of text to a file using Files class; closes stream after writing.
Files.write(Paths.get("test2.txt"), 
Arrays.asList("Hello","Béla"), 
Charset.defaultCharset(), 
StandardOpenOption.WRITE);
```
- Reading
```java
//Create BufferedReader using Files class
BufferedReader br=Files.newBufferedReader(Paths.get("test.txt"), 
Charset.defaultCharset());
//Read using while or foreach
String l;
while ((l = br.readLine()) != null) {
	System.out.println(l);
}
for (Object object : br.lines().toArray()) {
	System.out.println(object);
}
//Read using Stream API
br.lines().forEach(s->System.out.println(s));
//Close 
BufferedReader br.close();
//Read all lines of text from a file using Files class; closes stream after reading.
var lines=Files.readAllLines(Paths.get("test2.txt"), Charset.defaultCharset());
for (String line : lines) {
	 System.out.println(line);
 }
```
## Collections
- ArrayList
```java
//dinamikus lista
ArrayList<Person> aList=new ArrayList();
aList.add(joe);
for (Person person : aList) {
	System.out.println(person.getName());
}
//fix méretü
List<Person> test=Arrays.asList(joe, jane, tom, mary);
```
- PriorityQueue: an ordered queue (not FIFO)
	- add() only works if the correct order can be determined (elements must be Comparable)
```java
PriorityQueue<Person> queue=new PriorityQueue();
//Adding a non-comparable element throws ClassCastException
queue.add(jane);
//Peek: read head element
System.out.println(queue.peek().getName());
//Change head element priority
queue.peek().setAge(70);
 //Remove and reinsert
 queue.add(queue.poll())
 //Poll: read and remove head element
 while ((p=queue.poll())!=null) {
	System.out.printf("%s (%d)%n",p.getName(), p.getAge());
}
````
- LinkedList
```java
LinkedList<Person> lList=new LinkedList();
lList.add(joe);
//Elements can be added at a specific index like in a List
lList.add(1, mary)
//It can be used as a Queue (no priority here, just simple FIFO)
System.out.println("Peek: "+lList.peek().getName());
while ((p=lList.poll())!=null) {
	System.out.printf("%s (%d)%n",p.getName(), p.getAge());
}
```
- TreeSet
	- TreeSet is a sorted set (elements are ordered, duplications are not allowed)
```java
TreeSet<Person> tSet=new TreeSet();
tSet.add(joe);
```
- HashMap
```java
HashMap<Integer,Person> hMap=new HashMap();
hMap.put(111, joe);
for (Map.Entry<Integer, Person> entry : hMap.entrySet()) {
	System.out.printf("%d:%s%n",entry.getKey(),
	entry.getValue().getName());
}
//Get element value
System.out.println(hMap.get(112).getName());
```
# Stream API
```java
ArrayList<Person> aList=new ArrayList();
aList.add(new Person("Joe",25));
aList.stream().forEach(System.out::println); // :: is method reference operator
//Same as:
aList.stream().forEach(p->System.out.println(p))
//Convert stream back to List: 
collect(Collectors.toList())
//ordering
aList.stream().sorted(Comparator.comparing(Person::getAge)
	.reversed())
	.forEach(System.out::println);
//filtering
aList.stream().filter(p->p.getAge()>40).forEach(System.out::println);
//stream mapping
aList.stream().map(p->p.getName()).forEach(System.out::println);
//Names of people younger than 50, order by name. Collect names into a single string.
String names=aList.stream()
                .filter(p->p.getAge()<50)
                .map(p->p.getName())
                .sorted()
                .collect(Collectors.joining(", "));
//Aggregation
//Number of people whose names start with "J":
aList.stream().filter(p->p.getName().startsWith("J")).count()
//Youngest person:
aList.stream().min(Comparator.comparing(Person::getAge)).get();
//Average and sum age:
aList.stream().collect(Collectors.averagingInt(Person::getAge));
aList.stream().collect(Collectors.summingInt(Person::getAge));
collect(Collectors.toCollection(ArrayList::new)
.collect(Collectors.toList())

```
# Excpetions
- handle exception
```java
try {
	Hello("Shakespeare");
} catch (CoronaException ex) {
    System.out.println("Run you fools!");
```
- create custom exception
```java
public class CoronaException extends Exception{   
}
```
# Serialization
### XML
- object to file 
```java
//serlialize object to file
FileOutputStream fo=new FileOutputStream("test.txt");
ObjectOutputStream oos=new ObjectOutputStream(fo);
oos.writeObject(adam);
oos.close()
//from file to object
FileInputStream fi=new FileInputStream("test.txt");
ObjectInputStream ois=new ObjectInputStream(fi);
Person p=(Person)ois.readObject();
ois.close()
```
- XML serialization
	- XMLEncoder can serialize JavaBeans (no-argument constructor, getter&setter methods)
```java
//xml to file
FileOutputStream fo=new FileOutputStream("test.txt");
XMLEncoder xe=new XMLEncoder(fo);
xe.writeObject(adam);
xe.close()
//file to xml
FileInputStream fi=new FileInputStream("test.txt");
XMLDecoder xd=new XMLDecoder(fi);
Person p=(Person)xd.readObject();
xd.close()
```
- JAXB
	- Maven dependencies needed: 
		- jakarta.xml.bind: jakarta.xml.bind-api
		- com.sun.xml.bind: jaxb-impl
	- In pom.xml, click inside \<project>...\</project>, press Alt+Insert Dependency; Add both dependencies
	- If they won't be downloaded automatically: in Project tree, right click pom.xml, Maven/Reload project
- xml serlialize
	- Annotation necessary for the class: @XmlRootElement
	- JAXB serializes properties with getter&setter, or annotated fields: @XmlElement
```java 
JAXBContext ctx = JAXBContext.newInstance(Student.class);
Marshaller m = ctx.createMarshaller();
//If this property is true then Marshaller will create a nicely formatted, easily readable format.
m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
//Serialize to console
m.marshal(students.get(0), System.out);
//Serialize to text file
m.marshal(students.get(0), 
Files.newOutputStream(Paths.get("test2.txt"), StandardOpenOption.CREATE));
```
- xml deserlize
```java
Unmarshaller um=ctx.createUnmarshaller();
Object o=um.unmarshal(new File("test2.txt"));
System.out.println((Student)o);
```
### JSON
- using GSON
- maven: com.google.code.gson
- works also without getters and setters and default constructor
- serialize
```java
System.out.println(new Gson().toJson(students)); // compact
system.out.println(
                new GsonBuilder()
                        .setPrettyPrinting() // formatted output
                        .serializeNulls()    // save fields with null
                        .create()
                        .toJson(students));
//seralize to file
try (BufferedWriter out = 	Files.newBufferedWriter(Paths.get("test.json"))) {
	out.write(new Gson().toJson(students));
} catch (IOException x) { System.out.println(x.getMessage()); }
```
- deseralize
```java
// deserialize from file
try (BufferedReader in = 
Files.newBufferedReader(Paths.get("test.json"
))) {
	List<Student> newList = new
	Gson().fromJson(in, ArrayList.class);
    System.out.println("newlist: " + 
	newList);
} catch (IOException x) { 
System.out.println(x.getMessage()); }
```
# Webserver
- Tomcat: https://tomcat.apache.org/download-80.cgi
- core => zip => kicsomagolni
- File > New > Project... > Jakarta
- Project template: Web application 
- Application server: >  New... > Tomcat Server
- Next > Version: Jakarta EE 9 (Specifications: Servlet 5.0) > Finish (jakarta.* nem javax.*)
### Servlet
-  name url in anotation: @WebServlet(name = "WelcomeServlet", urlPatterns = {"/WelcomeServlet"})
- request and respons object: 
```java
protected void processRequest(HttpServletRequest request, 
HttpServletResponse response)
```
- response
	- response.setContentType("text/html;charset=UTF-8");
	- content:
		```java
		try (PrintWriter out = response.getWriter()) {  
 			out.println("<!DOCTYPE html>");  
 			out.println("<html>");
		```
- request
	- request.getParameter("add"); -> get  and post request
		- htmlben: ManageToppings?add=valami&
	- request.getRequestDispatcher("ManageToppings").forward(request, response); -> forward to site
### Webapp
- html files
- jsp files
### JSP
- session elérés: session....
- java code in block: \<%=i+1%\>
- expression: <%=%>
- import
	- \<%@page import="oe.pizzaapp.model.Pizza"%>  
- type	
	- \<%@page contentType="text/html" pageEncoding="UTF-8"%>
### Scopes
- request scope
	- request.getAttribute("sd")
	-  request.setAttribute("sd")
- global scope
	- getServletContext().getAttribute("appName") 
	- getServletContext().getAttribute("appName",valami) 
- session scope
	- request.getSession().setAttribute("pizza", newpizza); 
	- request.getSession().getAttribute("pizza");
	- request.getSession().getAttributeNames()
	- request.getSession().removeAttribute(String name)
- jsp
```java
<p>Request parameter:  
<%= request.getParameter("requestParamName")!=null ? request.getParameter("requestParamName"):"-" %>  
</p>  
<p>Request attribute:  
<%= request.getAttribute("requestAttrName")!=null ? request.getAttribute("requestAttrName"):"-" %>  
</p>  
<p>Session attribute:  
<%= session.getAttribute("sessionName")!=null ? session.getAttribute("sessionName"):"-" %>  
</p>  
<p>Application (context) attribute:  
<%= getServletContext().getAttribute("appName")!=null ? getServletContext().getAttribute("appName"):"-" %>  
</p>
```
## html
- Dokumentum felépítése
	```html
	<html>
		<head>
		<head>
	    <body>
	    </body>
    </html>
	```
- címsor:  \<h1>..\<h6>
-  bekezdés: \<p>
-  kisebb szövegrész megjelölése: \<span>
- kép: 
```html
<a href="https://get.adobe.com/flashplayer/">
	<img src="flash.png" alt="Flash Player letöltése"/>
</a>
```
- számozott lista: \<ol> 
- számozatlan lista: \<ul>
- listaelem: \<li>
- link: \<a>
- táblázat: \<table>
	- táblázat sor: \<tr>
	- táblázat cella: \<td>
	```html 
	<table border="1">
		\<tr> \<td>1\</td> \<td>2\</td> \</tr>
		\<tr> \<td>3\</td> \<td>4\</td> \</tr>
   \</table>
	```
- formok
```html
<form action="index.html" method="post">
	<!-- szövegmező -->
	<input type="text" name="username"/>
	<!-- számmező -->
	<input type="number" name="amount"/>
	<!-- jelszómező -->
	<input type="password" name="password"/>
	<!-- checkbox -->
 	<input type="checkbox" name="staysin" value="stay"/>Stay signed in
	<!-- rádiógomb csoport -->
 	<input type="radio" name="gender" value="male" /> Male<br/>
 	<input type="radio" name="gender" value="female" /> Female<br/>
	<!-- rejtett mező -->
 	<input type="hidden" name="hiddenvalue" value="YouNeverSeeThis" />
	<!-- lista -->
	<!-- többszörös kiválasztás: multiple="multiple" -->
 	<select name="youroption">
 		<option>One</option>
 		<option>Two</option>
 		<option>Three</option>
 	</select>
	<!-- gomb -->
 	<input type="submit" value="Login" />
</form>
```
```html
        <form action="NewOrder" method="post">
            <fieldset style="display:inline"><legend>Select size:</legend>
            <input type="radio" name="size" value="24"/>24 cm<br/>
            <input type="radio" name="size" value="32" checked="checked"/>32 cm<br/>
            <input type="radio" name="size" value="45"/>45 cm
            </fieldset><br/>
            <input type="submit" value="Next"/>
        </form>
```
