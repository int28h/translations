# Прерывания  
  
[Сурс](https://docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html)  
  
Прерывание — это указание для потока прекратить выполнение текущих операций и заняться чем-то иным. Программист решает, как именно поток отреагирует на прерывание, но как правило поток просто завершает свою работу. Такое использование прерываний подчеркивается в данном уроке.  
  
Поток вызывает прерывание с помощью вызова interrupt() для потока (т.е. для инстанса Thread или инстанса его дочернего класса), который нужно прервать. Для корректной работы прерываемый поток должен поддерживать механизм собственного прерывания.  

## Поддержка прерывания  
  
Как поток поддерживает собственное прерывание? Это зависит от того, чем он занят в текущий момент. Если в потоке часто вызваются методы, которые выбрасывают InterruptedException, то происходит возврат из метода run() после перехвата такого эксепшена. Например, предположим, что центральный цикл по сообщениям в примере SleepMessages находится в методе run() Runnable-объекта. Тогда он может быть изменен для поддержки прерываний следующим образом:  
  
	for (int i = 0; i < importantInfo.length; i++) {
		// Pause for 4 seconds
		try {
			Thread.sleep(4000);
		} catch (InterruptedException e) {
			// We've been interrupted: no more messages.
			return;
		}
		// Print a message
		System.out.println(importantInfo[i]);
	}
  
Many methods that throw InterruptedException, such as sleep, are designed to cancel their current operation and return immediately when an interrupt is received.  
  
What if a thread goes a long time without invoking a method that throws InterruptedException? Then it must periodically invoke Thread.interrupted, which returns true if an interrupt has been received. For example:  
  
	for (int i = 0; i < inputs.length; i++) {
		heavyCrunch(inputs[i]);
		if (Thread.interrupted()) {
			// We've been interrupted: no more crunching.
			return;
		}
	}
  
In this simple example, the code simply tests for the interrupt and exits the thread if one has been received. In more complex applications, it might make more sense to throw an InterruptedException:  
  
	if (Thread.interrupted()) {
		throw new InterruptedException();
	}
  
This allows interrupt handling code to be centralized in a catch clause.  
  
## The Interrupt Status Flag  
  
The interrupt mechanism is implemented using an internal flag known as the interrupt status. Invoking Thread.interrupt sets this flag. When a thread checks for an interrupt by invoking the static method Thread.interrupted, interrupt status is cleared. The non-static isInterrupted method, which is used by one thread to query the interrupt status of another, does not change the interrupt status flag.  
  
By convention, any method that exits by throwing an InterruptedException clears interrupt status when it does so. However, it's always possible that interrupt status will immediately be set again, by another thread invoking interrupt.  
  