#include <iostream>
#include <vector>
#include <thread>
#include <chrono>
#include <algorithm>
#include <SFML/Audio.hpp>
#include <SFML/Graphics.hpp>

using namespace std;
using namespace sf;

// Note mapping: MIDI note number to frequency
const vector<float> noteFrequencies = {
    // A0 to C8
    27.50, 29.14, 30.87, 32.70, 34.65, 36.71, 38.89, 41.20, 43.65, 46.25, 49.00, 51.91, 55.00, 58.27,
    61.74, 65.41, 69.30, 73.42, 77.78, 82.41, 87.31, 92.50, 98.00, 103.83, 110.00, 116.54, 123.47, 130.81,
    138.59, 146.83, 155.56, 164.81, 174.61, 185.00, 196.00, 207.65, 220.00, 233.08, 246.94, 261.63, 277.18,
    293.66, 311.13, 329.63, 349.23, 369.99, 392.00, 415.30, 440.00, 466.16, 493.88, 523.25, 554.37, 587.33,
    622.25, 659.26, 698.46, 739.99, 783.99, 830.61, 880.00, 932.33, 987.77, 1046.50, 1108.73, 1174.66,
    1244.51, 1318.51, 1396.91, 1479.98, 1567.98, 1661.22, 1760.00, 1864.66, 1975.53, 2093.00
};

// Function to play a note
void playNote(int noteIndex, float duration) {
    if (noteIndex >= 0 && noteIndex < noteFrequencies.size()) {
        SoundBuffer buffer;
        buffer.loadFromMemory(
            (const unsigned char*)"\x49\x44\x33\x03\x00\x00\x00\x00\x01\x00\x01\x00\x40\x10\x00\x00\xff\xff\xff\xff\x01\x00\x00\x00\x20\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x49\x44\x33\x03\x00\x00\x00\x00\x02\x00\x00\x00\x80\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x49\x44\x33\x03\x00\x00\x00\x00\x01\x00\x00\x00\x40\x10\x00\x00\xff\xff\xff\xff\x01\x00\x00\x00\x20\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x49\x44\x33\x03\x00\x00\x00\x00\x02\x00\x00\x00\x80\x10\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00",
            buffer.getMaximumSize()
        );
        Sound sound;
        sound.setBuffer(buffer);
        sound.setPitch(noteFrequencies[noteIndex] / 440.0f); // Adjust pitch
        sound.play();
        this_thread::sleep_for(chrono::milliseconds(static_cast<int>(duration * 1000)));
        sound.stop();
    }
}

// Function to draw piano keys
void drawPianoKeys(RenderWindow& window) {
    RectangleShape whiteKey(Vector2f(40, 100));
    RectangleShape blackKey(Vector2f(20, 60));
    whiteKey.setFillColor(Color::White);
    blackKey.setFillColor(Color::Black);

    // White keys
    int keyIndex = 0;
    for (int i = 0; i < 52; ++i) {
        whiteKey.setPosition(Vector2f(i * 40, 200));


        window.draw(whiteKey);
        if (i == 2 || i == 6 || i == 9 || i == 13 || i == 16 || i == 20 || i == 23 || i == 27 || i == 30 || i == 34 || i == 37 || i == 41 || i == 44 || i == 48) {
            keyIndex++;
        }
    }
    // Black keys
    for (int i = 0; i < 36; ++i) {
        blackKey.setPosition(Vector2f(i * 40 + 20 + 10, 140));
        window.draw(blackKey);
    }
}

int main() {
    RenderWindow window(VideoMode(800, 600), "Digital Piano");
    window.setFramerateLimit(60);

    while (window.isOpen()) {
        Event event;
        while (window.pollEvent(event)) {
            if (event.type == Event::Closed) {
                window.close();
            }
            if (event.type == Event::KeyPressed) {
                if (event.key.code == Keyboard::A) {
                    playNote(0, 0.5);
                } else if (event.key.code == Keyboard::S) {
                    playNote(2, 0.5);
                } else if (event.key.code == Keyboard::D) {
                    playNote(4, 0.5);
                } else if (event.key.code == Keyboard::F) {
                    playNote(5, 0.5);
                } else if (event.key.code == Keyboard::G) {
                    playNote(7, 0.5);
                } else if (event.key.code == Keyboard::H) {
                    playNote(9, 0.5);
                } else if (event.key.code == Keyboard::J) {
                    playNote(11, 0.5);
                } else if (event.key.code == Keyboard::K) {
                    playNote(12, 0.5);
                } else if (event.key.code == Keyboard::L) {
                    playNote(14, 0.5);
                }
            }
        }

        window.clear(Color::Black);
        drawPianoKeys(window);
        window.display();
    }

    return 0;
}
