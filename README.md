import java.util.Stack;

class TextEditor {
    private StringBuilder currentText = new StringBuilder();
    private Stack<String> undoStack = new Stack<>();
    private Stack<String> redoStack = new Stack<>();
    
    public void show() {
        System.out.println("Isi teks saat ini:");
        System.out.println(currentText.length() > 0 ? currentText.toString() : "(Kosong)");
    }

    // Fungsi untuk  menambahkan teks baru
    public void write(String text) {
        undoStack.push(currentText.toString());  // Simpan state untuk undo
        currentText.append(text);
        redoStack.clear();  // Kosongkan redo stack saat menulis teks baru
        System.out.println("Menulis: " + text);
    }

    // Fungsi untuk mengembalikan teks ke state sebelumnya (undo)
    public void undo() {
        if (!undoStack.isEmpty()) {
            redoStack.push(currentText.toString());  // Simpan state untuk redo
            currentText = new StringBuilder(undoStack.pop());
            System.out.println("Undo dilakukan.");
        } else {
            System.out.println("Tidak ada yang bisa di-undo.");
        }
    }

    // Fungsi untuk memulihkan teks ke state yang lebih baru (redo)
    public void redo() {
        if (!redoStack.isEmpty()) {
            undoStack.push(currentText.toString());  // Simpan state untuk undo
            currentText = new StringBuilder(redoStack.pop());
            System.out.println("Redo dilakukan.");
        } else {
            System.out.println("Tidak ada yang bisa di-redo.");
        }
    }
}

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        TextEditor textEditor = new TextEditor();
        boolean running = true;

        System.out.println("Menu Text Editor:");
        System.out.println("1. Show Text");
        System.out.println("2. Write Text");
        System.out.println("3. Undo");
        System.out.println("4. Redo");
        System.out.println("5. Exit");

        while (running) {
            System.out.print("\nPilih opsi: ");
            int choice = input.nextInt();
            input.nextLine();  // Consume newline

            switch (choice) {
                case 1:
                    textEditor.show();
                    break;
                case 2:
                    System.out.print("Masukkan teks yang ingin ditulis: ");
                    String text = input.nextLine();
                    textEditor.write(text);
                    break;
                case 3:
                    textEditor.undo();
                    break;
                case 4:
                    textEditor.redo();
                    break;
                case 5:
                    running = false;
                    System.out.println("Keluar dari program.");
                    break;
                default:
                    System.out.println("Opsi tidak valid. Silakan coba lagi.");
            }
        }

        input.close();
    }
}
