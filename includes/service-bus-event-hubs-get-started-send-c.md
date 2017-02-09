## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Ereignis Hubs

In diesem Abschnitt werden wir eine app C, um Ereignisse an Ihre Ereignis Hub zu senden, schreiben. Wir verwenden die Bibliothek Proton AMQP aus dem [Apache Qpid Projekt](http://qpid.apache.org/). Dies ist analog zur Verwendung von Servicebuswarteschlangen und Themen mit AMQP von C als angezeigte [hier](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Weitere Informationen finden Sie unter [Qpid Proton Dokumentation](http://qpid.apache.org/proton/index.html).

1. Klicken Sie von der [Seite Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html)auf den Link **Der Installation von Qpid Proton** aus, und folgen Sie den Anweisungen abhängig von Ihrer Umgebung. Es wird eine Linux-Umgebung davon ausgegangen; Angenommen, eine [Azure Linux virtueller Computer](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04.

2. Um die Bibliothek Proton kompilieren, installieren Sie die folgenden Pakete:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Herunterladen Sie der [Qpid Proton](http://qpid.apache.org/proton/index.html) Library und extrahieren Sie, z. B.:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Erstellen Sie ein Verzeichnis erstellen, kompilieren und installieren:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. Erstellen Sie eine neue Datei mit **sender.c** mit dem folgenden Inhalt in Ihrem Verzeichnis Arbeit. Denken Sie daran, ersetzen Sie den Wert für Ihre Veranstaltung Hub und einen Namen Namespace (letztere ist in der Regel `{event hub name}-ns`). Sie müssen für den zuvor erstellten **SendRule** durch eine URL-codierte Version des Schlüssels ersetzen. Sie können die URL codiert sie [hier](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Kompilieren Sie die Datei, die unter der Voraussetzung **Gcc**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] In diesem Code verwenden wir ein ausgehenden 1-Fenster, um die Nachrichten, so früh wie möglich erzwingen. Im Allgemeinen sollten Ihrer Anwendung Stapel Nachrichten Durchsatz zu erhöhen. Finden Sie unter [Qpid AMQP Messenger-Seite](http://qpid.apache.org/components/messenger/index.html) für Weitere Informationen zum Verwenden der Bibliothek Qpid Proton und die anderen Umgebungen und von Plattformen für die Bindungen bereitgestellt werden (aktuell Perl, PHP, Python und Ruby).
