
 @ProgramName: Program-1
 @Author: Arshia Clare 
 @Description: 
 This program reads in images stored as rgb values in a space delimited file format. There are functions that let the user manipulate the image to flip vertically and horizontally, and turn the image to grayscale.
 @Course: 1063 Data Structures
 @Semester: Spring 2017
 @Date: 09 02 2018 



 TXT Image Manipulation Starter
 
 This code is a simple way to read in color information stored in a space
 delimited txt format. The expected file format is:
                ---------------------------
                | width height            |
                | R G B R G B R G B R G B |
                | R G B R G B R G B R G B |
                | R G B R G B R G B R G B |
                | R G B R G B R G B R G B |
                | R G B R G B R G B R G B |
                | R G B R G B R G B R G B |
                ---------------------------
 So a 10x10 img would have 11 total rows, 10 rows of data, with 30 values in row.


#include<iostream>
#include<fstream>
#include<math.h>

using namespace std;

Structure to hold an rgb value

struct rgb{
    int r;
    int g;
    int b;
};


 @FunctionName: grayScale
 @Description: 
     Loops through a 2D array and turns every RGB value into its grayscale equivalent.
 @Params:
    rgb** image - 2D array holding rgb values
    int width - width of image
    int height - height of image
 @Returns:
    void

void grayScale(rgb** image,int width,int height){
    int r,g,b,gray;
    for(int i=0;i<height;i++){
        for(int j=0;j<width;j++){
            r = image[i][j].r;
            g = image[i][j].g;
            b = image[i][j].b;
            
            gray = (r+g+b)/3;
            
            image[i][j].r = gray;
            image[i][j].g = gray;
            image[i][j].b = gray;
        }
    }
}


 @FunctionName: flipVert
 @Description: 
     Loops through a 2D array and flips an image vertically as if you were folding the image in half from top to bottom or bottom to top.
 @Params:
    rgb** image - 2D array holding rgb values
    int width - width of image
    int height - height of image
 @Returns:
    void

void flipVert(rgb** image,int width,int height)
{
  int i, k , j;
    rgb** temp = new rgb*[height];
    for(int i=0;i<height;i++){
        temp[i] = new rgb[width]; //Now allocate each row of rgb's
    }
    //this loop does the flipping
    for(i = 0 , k= height-1; i < height/2, k > height/2; i++, k--)
    {
      for(j =0 ; j < width; j++)
      {
        temp[i][j] = image[i][j];
        image[i][j] = image[k][j];
        image[k][j] = temp[i][j];
      }
    }
}


 @FunctionName: flipHorz
 @Description: 
     Loops through a 2D array and flips an image horizontally as if you were folding the image in half from left to right or vice to versa.
 @Params:
    rgb** image - 2D array holding rgb values
    int width - width of image
    int height - height of image
 @Returns:
    void

void flipHorz(rgb** image,int width,int height)
{
  int g, k, j;
    rgb** temp = new rgb*[height];
    for(int i=0;i<height;i++)
    {
        temp[i] = new rgb[width]; //Now allocate each row of rgb's
    }
    for (g = 0, k = width-1; g < width/2 , k >= width/2; g++, k--)
    {
      for (j = 0; j < height; j++)
      {
        temp[j][g] = image[j][g];
        image[j][g] = image[j][k];
        image[j][k] = temp[j][g];
      }
    }
}

int main()
{
    ifstream ifile;          //Input / output files
    ofstream ofile;
    ifile.open("apple.txt");
    ofile.open("appleMix.txt");   
    
    int width;               //width of image
    int height;              //height of image
    
    rgb **imgArray;         //Pointer var for our 2D array because we         
                            //don't know how big the image will be!

    ifile>>width>>height;   //Read in width and height from top of input file
                            //We need this so we can make the array the right 
                            //size. After we get these two values, we can
                            //now allocate memory for our 2D array.

    imgArray = new rgb*[height];    //This array points to every row

    for(int i=0;i<height;i++){
        imgArray[i] = new rgb[width]; //Now allocate each row of rgb's
    }
    
    //Read the color data in from our txt file
    for(int i=0;i<height;i++){
        for(int j=0;j<width;j++){
            ifile>>imgArray[i][j].r>>imgArray[i][j].g>>imgArray[i][j].b;            
        }
    }
    
    //We could make any changes we want to the color image here
    grayScale(imgArray,width,height);
    
    flipVert(imgArray,width,height);
    
    flipHorz(imgArray,width,height);
    
    //Write out our color data to a new file
    ofile<<width<<" "<<height<<endl;
    for(int i=0;i<height;i++){
        for(int j=0;j<width;j++){
            ofile<<imgArray[i][j].r<<" "<<imgArray[i][j].g<<" "<<imgArray[i][j].b<<" ";
        }
        ofile<<endl;
    }   
  return 0;
}
