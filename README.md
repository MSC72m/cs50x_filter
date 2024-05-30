# Image Processing with Filters

## Project Description

This project includes several functions to apply different filters to an image. The filters include grayscale, sepia, horizontal reflection, and blur. These functions are designed to manipulate images by adjusting the RGB values of each pixel according to specific algorithms.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Code Explanation](#code-explanation)

## Installation

No additional libraries are required beyond the standard C library.

## Usage

To compile and run the program, use the following commands:
```bash
make filters
./filters
```
## Code Explanation
Header File Inclusion and Libraries
``` C
#include "helpers.h"
#include <math.h>
#include <string.h>
#include "helpers.h"
```
includes the header file where the RGBTRIPLE struct is defined. #include <math.h> and #include <string.h> include standard libraries for mathematical functions and string manipulation, respectively.

Grayscale Function
``` C
// Convert image to grayscale
void grayscale(int height, int width, RGBTRIPLE image[height][width])
{
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            int average = round((image[i][j].rgbtRed + image[i][j].rgbtBlue + image[i][j].rgbtGreen) / 3.0);
            image[i][j].rgbtRed = average;
            image[i][j].rgbtBlue = average;
            image[i][j].rgbtGreen = average;
        }
    }
    return;
}
```
The grayscale function converts each pixel of the image to a shade of gray. It calculates the average of the red, green, and blue values and sets each color component of the pixel to this average.

Sepia Function
``` C
// Convert image to sepia
void sepia(int height, int width, RGBTRIPLE image[height][width])
{
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            int sepiaRed = round(image[i][j].rgbtRed * 0.393 + image[i][j].rgbtGreen * 0.769 + image[i][j].rgbtBlue * 0.189);
            int sepiaGreen = round(image[i][j].rgbtRed * 0.349 + image[i][j].rgbtGreen * 0.686 + image[i][j].rgbtBlue * 0.168);
            int sepiaBlue = round(image[i][j].rgbtRed * 0.272 + image[i][j].rgbtGreen * 0.534 + image[i][j].rgbtBlue * 0.131);

            if (sepiaRed > 255)
            {
                sepiaRed = 255;
            }
            if (sepiaGreen > 255)
            {
                sepiaGreen = 255;
            }
            if (sepiaBlue > 255)
            {
                sepiaBlue = 255;
            }

            image[i][j].rgbtRed = sepiaRed;
            image[i][j].rgbtGreen = sepiaGreen;
            image[i][j].rgbtBlue = sepiaBlue;
        }
    }
    return;
}
```
The sepia function converts each pixel of the image to a sepia tone. It uses specific weights for the red, green, and blue values to create the sepia effect and ensures that the values do not exceed 255.

Reflect Function
``` C
// Reflect image horizontally
void reflect(int height, int width, RGBTRIPLE image[height][width])
{
    RGBTRIPLE newImage[height][width];
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            newImage[i][j] = image[i][j];
        }
    }
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width / 2; j++)
        {
            RGBTRIPLE temp = image[i][j];
            image[i][j] = newImage[i][width - 1 - j];
            image[i][width - 1 - j] = temp;
        }
    }
    return;
}
```
The reflect function flips the image horizontally. It creates a copy of the original image, then swaps pixels from the left side with pixels from the right side.

Blur Function
``` C
// Blur image
void blur(int height, int width, RGBTRIPLE image[height][width])
{
    RGBTRIPLE newImage[height][width];
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            newImage[i][j] = image[i][j];
        }
    }

    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            int count = 0;
            int redSum = 0, blueSum = 0, greenSum = 0;

            for (int row = -1; row <= 1; row++)
            {
                for (int col = -1; col <= 1; col++)
                {
                    if (i + row >= 0 && i + row < height && j + col >= 0 && j + col < width)
                    {
                        redSum += newImage[i + row][j + col].rgbtRed;
                        blueSum += newImage[i + row][j + col].rgbtBlue;
                        greenSum += newImage[i + row][j + col].rgbtGreen;
                        count++;
                    }
                }
            }

            image[i][j].rgbtRed = round((double) redSum / count);
            image[i][j].rgbtBlue = round((double) blueSum / count);
            image[i][j].rgbtGreen = round((double) greenSum / count);
        }
    }
    return;
}
```
The blur function applies a blur effect to the image. It creates a copy of the original image, and then averages the colors of each pixel with its surrounding pixels.
