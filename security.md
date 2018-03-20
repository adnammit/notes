# SECURITY


* authorization vs authentication

* types of attacks:
    - DDoS: Distributed Denial of Service (DDoS)
        * an attempt to make an online service unavailable by overwhelming it with traffic from multiple sources
    - MITM: Man in the Middle:
        * an attack where the attacker secretly relays and possibly alters the communication between two parties
        * they believe they are directly communicating with each other
        * example: eavesdropping, in which the attacker makes independent connections with the victims and relays messages between them to make them believe they are talking directly to each other over a private connection, when in fact the entire conversation is controlled by the attacker
    - SQLi: SQL injection
        * user input is added directly to a SQL query without being sanitized
        ```
        passwd="password’ OR 1=1"
        sql = “SELECT id FROM users WHERE username=’” + uname + “’ AND password=’” + passwd + “’”
        database.execute(sql) // attack results in authentication bypass
        ```
        * using SQLi, an attacker can:
            - bypass authentication
            - access data within the database
            - corrupt or delete the database
            - use a SQLi as the initial vector in an attack of an internal server behind a firewall

(XSS: cross site scripting, injecting client-side script into a web-page)
