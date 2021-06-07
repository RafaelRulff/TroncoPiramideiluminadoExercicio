# TroncoPiramideiluminadoExercicio

public class MyRenderer implements Renderer {

private float xrot;             //X Rotation ( NEW )

private float yrot;             //Y Rotation ( NEW )

private float zrot;             //Z Rotation ( NEW )

/** The Activity Context ( NEW ) */

private Context context;

private Pyramid pyramid;

/**
 * Instance the Cube object and set 
 * 
 * the Activity Context handed over
 * 
 */
 
 void main()
{
    float ambientStrength = 0.1;
    
    vec3 ambient = ambientStrength * lightColor;

    vec3 result = ambient * objectColor;
    
    FragColor = vec4(result, 1.0);
}  

#version 330 core
layout (location = 0) in vec3 aPos;

layout (location = 1) in vec3 aNormal;

glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)0);

glEnableVertexAttribArray(0);

out vec3 Normal;

void main()
{
    gl_Position = projection * view * model * vec4(aPos, 1.0);
    Normal = aNormal;
} 

in vec3 Normal; 

uniform vec3 lightPos; 

lightingShader.setVec3("lightPos", lightPos);


out vec3 FragPos;  

out vec3 Normal;
  
void main()
{
    gl_Position = projection * view * model * vec4(aPos, 1.0);
    
    FragPos = vec3(model * vec4(aPos, 1.0));
    
    Normal = aNormal;
}

in vec3 FragPos;  

vec3 norm = normalize(Normal);

vec3 lightDir = normalize(lightPos - FragPos); 

public MyRenderer(Context context) {

float diff = max(dot(norm, lightDir), 0.0);

vec3 diffuse = diff * lightColor;

vec3 result = (ambient + diffuse) * objectColor;

FragColor = vec4(result, 1.0);

uniform vec3 viewPos;


lightingShader.setVec3("viewPos", camera.Position); 

float specularStrength = 0.5;

vec3 viewDir = normalize(viewPos - FragPos);

vec3 reflectDir = reflect(-lightDir, norm); 

loat spec = pow(max(dot(viewDir, reflectDir), 0.0), 32);

vec3 result = (ambient + diffuse + specular) * objectColor;

FragColor = vec4(result, 1.0);

vec3 specular = specularStrength * spec * lightColor;  


    this.context = context;

    pyramid = new Pyramid(this.context);

}

/**
 * The Surface is created/init()
 */
public void onSurfaceCreated(GL10 gl, EGLConfig config) {       

    //Load the texture for the cube once during Surface creation


    gl.glEnable(GL10.GL_TEXTURE_2D);            //Enable Texture Mapping ( NEW )
    
    gl.glShadeModel(GL10.GL_SMOOTH);            //Enable Smooth Shading
    
    gl.glClearColor(1.0f, 1.0f, 1.0f, 0.5f);    //Black Background
    
    gl.glClearDepthf(1.0f);                     //Depth Buffer Setup
    
    gl.glEnable(GL10.GL_DEPTH_TEST);            //Enables Depth Testing
    
    gl.glDepthFunc(GL10.GL_LEQUAL);             //The Type Of Depth Testing To Do

    //Really Nice Perspective Calculations
    gl.glHint(GL10.GL_PERSPECTIVE_CORRECTION_HINT, GL10.GL_NICEST); 
}

public void onDrawFrame(GL10 gl) {

    //Clear Screen And Depth Buffer
    
    gl.glClear(GL10.GL_COLOR_BUFFER_BIT | GL10.GL_DEPTH_BUFFER_BIT);   
    
    gl.glLoadIdentity();                    //Reset The Current Modelview Matrix

    //Drawing
    
    gl.glTranslatef(0.0f, -1.0f, -5.0f);        //Move 5 units into the screen
    
    gl.glScalef(1.0f, 1.0f, 1.0f);          //Scale the Cube to 80 percent, otherwise it would be too large for the screen

    //Rotate around the axis based on the rotation matrix (rotation, x, y, z)
    
     gl.glRotatef(yrot, 0.0f, 1.65f, 0.0f); //X

     pyramid.draw(gl, context);

     yrot += 1.0f;

}

/**
 * If the surface changes, reset the view
 */

public void onSurfaceChanged(GL10 gl, int width, int height) {

    if(height == 0) {                       //Prevent A Divide By Zero By
    
        height = 1;                         //Making Height Equal One
        
    }

    gl.glViewport(0, 0, width, height);     //Reset The Current Viewport
    
    gl.glMatrixMode(GL10.GL_PROJECTION);    //Select The Projection Matrix
    
    gl.glLoadIdentity();                    //Reset The Projection Matrix

    //Calculate The Aspect Ratio Of The Window
    
    GLU.gluPerspective(gl, 45.0f, (float)width / (float)height, 0.1f, 100.0f);

    gl.glMatrixMode(GL10.GL_MODELVIEW);     //Select The Modelview Matrix
    
    gl.glLoadIdentity();                    //Reset The Modelview Matrix
    
 }
}

public class PyramidNew {

   int []texturesID = new int[3];

   Bitmap []bitmap = new Bitmap[3];

  private FloatBuffer textureBuffer;

   private FloatBuffer vertexBuffer;  // Buffer for vertex-array

   private float[][] colors = {  // Colors of the 6 faces
   
      {1.0f, 0.5f, 0.0f, 1.0f},  // 0. orange
      
      {1.0f, 0.0f, 1.0f, 1.0f},  // 1. violet
      
      {0.0f, 1.0f, 0.0f, 1.0f},  // 2. green
      
      {0.0f, 0.0f, 1.0f, 1.0f},  // 3. blue
      
      {1.0f, 0.0f, 0.0f, 1.0f},  // 4. red
      
      {1.0f, 1.0f, 0.0f, 1.0f}   // 5. yellow
   };

/*   private float[] vertices = {  // Vertices for the front face

      -1.5f,  0.0f, 0.86f,  // 0. left-bottom-front
      
       1.5f,  0.0f, 0.86f,  // 1. right-bottom-front
       
       0.0f,  1.86f, 0.0f,  // 2. left-top-front
       
         // 3. right-top-front
         
   };*/
   
   private float[] vertices = {  // Vertices for the front face
   
              -1.0f,  0.0f, 0.86f,  // 0. left-bottom-front
              
               1.0f,  0.0f, 0.86f,  // 1. right-bottom-front
               
               0.0f,  1.86f, 0.0f,  // 2. left-top-front
               
                 // 3. right-top-front
           };
   private float textures[] = {         
   
            //Mapping coordinates for the vertices
            
            0.0f, 0.0f,
            
            0.0f, 1.0f,
            
            1.0f, 1.0f,

    };

   // Constructor - Set up the buffers
   
   public PyramidNew( Context context) {
   
     // Setup vertex-array buffer. Vertices in float. An float has 4 bytes
     
      System.out.println("calling Pyramid:::::::::");
      
      ByteBuffer vbb = ByteBuffer.allocateDirect(vertices.length * 4);
      
      vbb.order(ByteOrder.nativeOrder()); // Use native byte order
      
      vertexBuffer = vbb.asFloatBuffer(); // Convert from byte to float
      
      vertexBuffer.put(vertices);         // Copy data into buffer
      
      vertexBuffer.position(0);           // Rewind

    ByteBuffer byteBuf = ByteBuffer.allocateDirect(textures.length * 4);
    
    byteBuf.order(ByteOrder.nativeOrder());
    
    textureBuffer = byteBuf.asFloatBuffer();
    
    textureBuffer.put(textures);
    
    textureBuffer.position(0);


     InputStream stream1 = context.getResources().openRawResource(R.drawable.splash_screen);
     
     InputStream stream2 = context.getResources().openRawResource(R.drawable.bg);
     
     InputStream stream3 = context.getResources().openRawResource(R.drawable.bg1);
     


      try {
            //BitmapFactory is an Android graphics utility for images

            bitmap[0] = BitmapFactory.decodeStream(stream1);
            
            bitmap[1] = BitmapFactory.decodeStream(stream2);
            
            bitmap[2] = BitmapFactory.decodeStream(stream3);

        } finally {
        
            //Always clear and close
            
            try {
                stream1.close();
                
                stream2.close
                
                stream3.close();

                stream1 = stream2 = stream3 = null;
                
            } catch (IOException e) {
            
            }
        }


   }

   // Draw the color cube
   
   public void draw(GL10 gl, Context context) {
   
      gl.glFrontFace(GL10.GL_CCW);    // Front face in counter-clockwise orientation
      
      gl.glEnable(GL10.GL_CULL_FACE); // Enable cull face
      
      gl.glCullFace(GL10.GL_BACK);    // Cull the back face (don't display)

      gl.glEnableClientState(GL10.GL_VERTEX_ARRAY);
      
      gl.glVertexPointer(3, GL10.GL_FLOAT, 0, vertexBuffer);
      
      gl.glEnableClientState(GL10.GL_TEXTURE_COORD_ARRAY);


      //loading the first texture in first face
      
      loadTexture(gl, context, 0);
      
      gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 0, 3);

      // rotating the face and puting the second texture in face
      
      gl.glRotatef(120.0f, 0.0f, 1.0f, 0.0f);
      
      //gl.glColor4f(colors[1][0], colors[1][1], colors[1][2], colors[1][3]);
      
      loadTexture(gl, context, 1);
      
      gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 0, 3);

      // Back - Rotate another 120 degree about y-axis and then put different texture
      
      gl.glRotatef(120.0f, 0.0f, 1.0f, 0.0f);
      
      //gl.glColor4f(colors[2][0], colors[2][1], colors[2][2], colors[2][3]);
      
      loadTexture(gl, context, 2);
      
      gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 0, 3);

      gl.glDisableClientState(GL10.GL_VERTEX_ARRAY);
      
      gl.glDisable(GL10.GL_CULL_FACE);
   }


   public void loadTexture(GL10 gl, Context context, int currentImage) {

        //  Bitmap []bitmap = new Bitmap[3];

          gl.glGenTextures(3, texturesID, 0); // Generate texture-ID array for 6 IDs

          gl.glBindTexture(GL10.GL_TEXTURE_2D, texturesID[currentImage]);

          // Generate OpenGL texture images

       // Create Nearest Filtered Texture
       
          gl.glTexParameterf(GL10.GL_TEXTURE_2D, GL10.GL_TEXTURE_MIN_FILTER,
          
                  GL10.GL_LINEAR);
                  
          gl.glTexParameterf(GL10.GL_TEXTURE_2D, GL10.GL_TEXTURE_MAG_FILTER,
          
                  GL10.GL_LINEAR);

          // Different possible texture parameters, e.g. GL10.GL_CLAMP_TO_EDGE
          
          gl.glTexParameterf(GL10.GL_TEXTURE_2D, GL10.GL_TEXTURE_WRAP_S,
          
                  GL10.GL_CLAMP_TO_EDGE);
                  
          gl.glTexParameterf(GL10.GL_TEXTURE_2D, GL10.GL_TEXTURE_WRAP_T,
          
                  GL10.GL_REPEAT);

             // Build Texture from loaded bitmap for the currently-bind texture ID

          GLUtils.texImage2D(GL10.GL_TEXTURE_2D, 0, bitmap[currentImage], 0);

            gl.glTexCoordPointer(2, GL10.GL_FLOAT, 0, textureBuffer);

            gl.glEnable(GL10.GL_TEXTURE_2D);

            // Enable the texture state
            
            gl.glEnableClientState(GL10.GL_TEXTURE_COORD_ARRAY);

            // Point to our buffers
            
            gl.glVertexPointer(3, GL10.GL_FLOAT, 0, vertexBuffer);



      }
}
