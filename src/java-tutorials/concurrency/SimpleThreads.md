# Пример: SimpleThreads  

[Сурс](https://docs.oracle.com/javase/tutorial/essential/concurrency/simple.html)  

Пример ниже содержит некоторые концепции, рассмотренные в этом разделе. SimpleThreads состоит из двух потоков. Первый - это основной поток, который есть в каждом Java-приложении. Основной поток создает новый поток из объекта класса, имплементирующего Runnable - MessageLoop - и ожидает его завершения. Если потоку MessageLoop требуется слишком много времени для завершения, основной поток прерывает его.  

Поток MessageLoop выводит набор сообщений. В случае получения прерывания до того, как MessageLoop напечатает все свои сообщения, он печатает соответствующее сообщение и завершается.

  public class SimpleThreads {

      // Выводит сообщение, перед которым прописывается имя текущего потока
      static void threadMessage(String message) {
          String threadName = Thread.currentThread().getName();
          System.out.format("%s: %s%n", threadName, message);
      }

      private static class MessageLoop implements Runnable {
          public void run() {
              String importantInfo[] = {
                  "Mares eat oats",
                  "Does eat oats",
                  "Little lambs eat ivy",
                  "A kid will eat ivy too"
              };
              try {
                  for (int i = 0;
                       i < importantInfo.length;
                       i++) {
                      // Четырёхсекундная пауза
                      Thread.sleep(4000);
                      // Печать сообщения
                      threadMessage(importantInfo[i]);
                  }
              } catch (InterruptedException e) {
                  threadMessage("I wasn't done!");
              }
          }
      }

      public static void main(String args[]) throws InterruptedException {

          // Задержка (в миллисекундах) перед тем как мы прервем поток MessageLoop (по умолчанию - один час)
          long patience = 1000 * 60 * 60;

          // Если время задержки передано в аргументах командной строки,
          // то оно перед применением переводится в миллисекунды
          if (args.length > 0) {
              try {
                  patience = Long.parseLong(args[0]) * 1000;
              } catch (NumberFormatException e) {
                  System.err.println("Argument must be an integer.");
                  System.exit(1);
              }
          }

          threadMessage("Starting MessageLoop thread");
          long startTime = System.currentTimeMillis();
          Thread t = new Thread(new MessageLoop());
          t.start();

          threadMessage("Waiting for MessageLoop thread to finish");
          // цикл до тех пор, пока поток MessageLoop существует
          while (t.isAlive()) {
              threadMessage("Still waiting...");
              // Ожидание длительностью максимум 1 секунду перед завершением потока MessageLoop 
              t.join(1000);
              if (((System.currentTimeMillis() - startTime) > patience) && t.isAlive()) {
                  threadMessage("Tired of waiting!");
                  t.interrupt();
                  // Shouldn't be long now
                  // -- wait indefinitely
                  t.join();
              }
          }
          threadMessage("Finally!");
      }
