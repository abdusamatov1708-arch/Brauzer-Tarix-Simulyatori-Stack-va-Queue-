# Brauzer-Tarix-Simulyatori-Stack-va-Queue-
class Stack {
    constructor() {
        this.items = [];
    }

    push(item) {
        this.items.push(item);
    }

    pop() {
        if (this.isEmpty()) {
            throw new Error("Stack bo'sh!");
        }
        return this.items.pop();
    }

    peek() {
        if (this.isEmpty()) {
            return null;
        }
        return this.items[this.items.length - 1];
    }

    isEmpty() {
        return this.items.length === 0;
    }

    clear() {
        this.items = [];
    }
}

class Queue {
    constructor() {
        this.items = [];
    }

    enqueue(item) {
        this.items.push(item);
    }

    dequeue() {
        if (this.isEmpty()) {
            throw new Error("Navbat bo'sh!");
        }
        return this.items.shift();
    }

    front() {
        if (this.isEmpty()) {
            return null;
        }
        return this.items[0];
    }

    isEmpty() {
        return this.items.length === 0;
    }
}

class BrowserHistory {
    constructor(homePage = "homepage.com") {
        this.currentUrl = homePage;
        this.backStack = new Stack();
        this.forwardStack = new Stack();
    }

    visit(url) {
        this.backStack.push(this.currentUrl);
        this.currentUrl = url;
        this.forwardStack.clear(); // Yangi sahifaga o'tganda forward tozalanadi
        console.log(`[Navigatsiya] Ochildi: ${url}`);
    }

    back() {
        if (this.backStack.isEmpty()) {
            throw new Error("Orqaga qaytish uchun sahifalar yo'q!");
        }
        this.forwardStack.push(this.currentUrl);
        this.currentUrl = this.backStack.pop();
        console.log(`[Orqaga] Hozirgi sahifa: ${this.currentUrl}`);
    }

    forward() {
        if (this.forwardStack.isEmpty()) {
            throw new Error("Oldinga o'tish uchun sahifalar yo'q!");
        }
        this.backStack.push(this.currentUrl);
        this.currentUrl = this.forwardStack.pop();
        console.log(`[Oldinga] Hozirgi sahifa: ${this.currentUrl}`);
    }

    showState() {
        console.log(`--- BRAUZER HOLATI ---`);
        console.log(`BackStack: [${this.backStack.items.join(", ")}]`);
        console.log(`Hozirgi URL: ${this.currentUrl}`);
        console.log(`ForwardStack: [${this.forwardStack.items.join(", ")}]`);
        console.log(`----------------------\n`);
    }
}

class DownloadQueue {
    constructor() {
        this.queue = new Queue();
    }

    enqueue(file) {
        this.queue.enqueue(file);
        console.log(`[Yuklash] Navbatga qo'shildi: ${file}`);
    }

    dequeue() {
        return this.queue.dequeue();
    }

    isEmpty() {
        return this.queue.isEmpty();
    }

    processNext() {
        if (this.isEmpty()) {
            console.log("[Yuklash] Yuklashlar navbati bo'sh.");
            return;
        }
        const file = this.dequeue();
        console.log(`[Yuklanmoqda...] Fayl muvaffaqiyatli yuklab olindi: ${file}`);
    }

    showQueue() {
        console.log(`[Yuklash Navbati]: [${this.queue.items.join(", ")}]`);
    }
}

// --- SINIQLASH (TEST QILISH) ---

console.log("=== 1-QISM & 3-QISM: BRAUZER NAVIGATSIYASI (STACK) ===");
const browser = new BrowserHistory("google.com");
browser.showState();

// Kamida 5 ta URLga tashrif buyurish
browser.visit("github.com");
browser.visit("stackoverflow.com");
browser.visit("youtube.com");
browser.visit("kun.uz");
browser.showState();

// Orqaga yurish
browser.back(); // youtube.com ga qaytadi
browser.back(); // stackoverflow.com ga qaytadi
browser.showState();

// Oldinga yurish
browser.forward(); // youtube.com ga o'tadi
browser.showState();

// Yangi sahifaga o'tish (forward stack tozalanishini tekshirish uchun)
browser.visit("wikipedia.org");
browser.showState();


console.log("=== 2-QISM & 3-QISM: YUKLAB OLISH NAVBATI (QUEUE) ===");
const downloads = new DownloadQueue();

// 3 ta yuklab olish qo'shish
downloads.enqueue("document.pdf");
downloads.enqueue("installer.exe");
downloads.enqueue("photo.jpg");
downloads.showQueue();

// Navbat bo'yicha yuklab olish simulyatsiyasi (FIFO)
downloads.processNext(); // document.pdf
downloads.processNext(); // installer.exe
downloads.showQueue();

downloads.processNext(); // photo.jpg
downloads.processNext(); // Navbat bo'sh
