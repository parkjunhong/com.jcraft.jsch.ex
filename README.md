# com.jcraft.jsch.ex

# original author
http://www.jcraft.com/jsch/

# source from
https://sourceforge.net/projects/jsch/files/jsch/0.1.55/

# Behind story for this repository.

Now latest version is 0.1.55 and I use this, but only [0.1.54](https://github.com/is/jsch) exists on Github. (2020-05-20)

__Found Bugs__\
Recently I've finished a program that provides a remote port forwarding.
This program provides 3 functions.
1. Connects a remote port forwarding.
2. Disconnects a remote port forwarding connection.
3. Lists remote port forwarding.

I found a bug that Remote Port Forwardings are mixed,
although it's remote server is different from each other.

A method that provides Local Port Forwarding checks a Session instance.

``` java
PortWatcher.java
static String[] getPortForwarding(Session session){
  java.util.Vector foo=new java.util.Vector();
 
  synchronized(pool){
    for(int i=0; i<pool.size(); i++){
      PortWatcher p=(PortWatcher)(pool.elementAt(i));
      if(p.session==session){
        foo.addElement(p.lport+":"+p.host+":"+p.rport);
      }
    }
  }
 
  String[] bar=new String[foo.size()];
  for(int i=0; i<foo.size(); i++){
    bar[i]=(String)(foo.elementAt(i));
  }
  return bar;
}
```
But a method that provides Remote Port Forwardings DO NOT CHECK a Session instance.

``` java
ChannelForwardedTCPIP.java
static String[] getPortForwarding(Session session){
  Vector foo = new Vector();
 
  synchronized(pool){
    for(int i=0; i<pool.size(); i++){
      Config config = (Config)(pool.elementAt(i));
      if(config instanceof ConfigDaemon)
        foo.addElement(config.allocated_rport+":"+config.target+":");
      else
        foo.addElement(config.allocated_rport+":"+config.target+":"+((ConfigLHost)config).lport);
    }
  }
 
  String[] bar=new String[foo.size()];
  for(int i=0; i<foo.size(); i++){
    bar[i]=(String)(foo.elementAt(i));
  }
  return bar;
}
```

---
# Repository
maven (**[Go to LATEST](http://nexus3.ymtech.co.kr/#browse/browse:maven-public:com%2Fjcraft%2Fjsch.ex)**)
``` xml
<repositories>
  <repository>
    <id>ymtech.kr</id>
    <name>YMTECH Maven Repository</name>
    <url>http://nexus3.ymtech.co.kr/repository/maven-public/</url>
    <layout>default</layout>
  </repository>
</repositories>

<dependency>
  <groupId>com.jcraft</groupId>
  <artifactId>jsch.ex</artifactId>
  <version>${com-jcraft-jsch-ex.version}</version>
</dependency>
```
