```java
package org.max;

import java.util.Arrays;

public class Main {
    private static final int WIDTH = 80;
    private static final int HEIGHT = 22;

    public static void main(String[] args) {
        double azimuth = 0, elevation = 0;
        double[] depthBuffer = new double[WIDTH * HEIGHT];
        char[] frameBuffer = new char[WIDTH * HEIGHT];

        System.out.print("\u001b[2J");

        while (true) {
            Arrays.fill(frameBuffer, ' ');
            Arrays.fill(depthBuffer, 0);

            renderScene(azimuth, elevation, depthBuffer, frameBuffer);

            System.out.print("\u001b[H");
            for (int k = 0; k < WIDTH * HEIGHT; k++) {
                System.out.print(k % WIDTH > 0 ? frameBuffer[k] : '\n');
            }

            azimuth += 0.04;
            elevation += 0.02;
        }
    }

    private static void renderScene(double azimuth, double elevation, double[] depthBuffer, char[] frameBuffer) {
        for (double j = 0; 6.28 > j; j += 0.07) {
            for (double i = 0; 6.28 > i; i += 0.02) {
                // ... (rest of your existing code)
                // Extracted the inner loop logic into a separate method for better readability
                renderPixel(i, j, azimuth, elevation, depthBuffer, frameBuffer);
            }
        }
    }

    private static void renderPixel(double i, double j, double azimuth, double elevation,
                                    double[] depthBuffer, char[] frameBuffer) {
        double c = Math.sin(i);
        double d = Math.cos(j);
        double e = Math.sin(azimuth);
        double f = Math.sin(j);
        double g = Math.cos(azimuth);
        double h = d + 2;
        double D = 1 / (c * h * e + f * g + 5);
        double l = Math.cos(i);
        double m = Math.cos(elevation);
        double n = Math.sin(elevation);
        double t = c * h * g - f * e;

        int x = (int) (40 + 30 * D * (l * h * m - t * n));
        int y = (int) (12 + 15 * D * (l * h * n + t * m));
        int o = x + WIDTH * y;
        int N = (int) (8 * ((f * e - c * d * g) * m - c * d * e - f * g - l * d * n));

        if (isWithinBounds(x, y) && D > depthBuffer[o]) {
            depthBuffer[o] = D;
            frameBuffer[o] = getAsciiChar(N);
        }
    }

    private static boolean isWithinBounds(int x, int y) {
        return y > 0 && y < HEIGHT && x > 0 && x < WIDTH;
    }

    private static char getAsciiChar(int index) {
        char[] asciiChars = {'.', ',', '-', '~', ':', ';', '=', '!', '*', '#', '$', '@'};
        return asciiChars[Math.max(index, 0)];
    }
}
```
