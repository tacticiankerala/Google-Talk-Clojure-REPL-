

(ns core (:gen-class))



(defn -disconn
  [con]
  (.disconnect con)
)
(defn -main [username password mailid ]
  (def conf (org.jivesoftware.smack.ConnectionConfiguration. "talk.google.com" 5222 "gmail.com") )
   (def con (org.jivesoftware.smack.XMPPConnection. conf))
   (org.jivesoftware.smack.SASLAuthentication/supportSASLMechanism "PLAIN" 0)
   (.connect con)
   (try (.login  con username password "resource") (catch Exception e [(println "Authentication error") (System/exit 0)])) 
   (if (.isAuthenticated con) (println "Connection Successfull") (println "Connection Failed"))
   (def chat (.createChat (.getChatManager con) mailid nil))
   (def ml (proxy [org.jivesoftware.smack.MessageListener] []  
      (processMessage
        [ch ms]
        (let [ s# (new java.io.StringWriter) content (. ms getBody)]
           (binding [*out* s#]
           (try (let [res (str (load-string content)) ] (when-not (= res "") (. ch sendMessage res))) 
                (catch Exception e (. ch sendMessage (str "Exception " (.getMessage e))))
           )
           (. ch sendMessage (let [msg (str s#)] (when-not (= msg "") msg)))
        )
   ))))

   (. chat addMessageListener ml)
   (while true nil)
  )
