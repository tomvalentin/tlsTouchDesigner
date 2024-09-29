# Introduction to TouchDesigner

Intro to TouchDesigner according to ChatGPT

## Key Concepts

### 1. Operators (OPs)
TouchDesigner uses different types of operators to process and manipulate data. There are five primary types of operators:

- **TOPs (Texture Operators):** Used for 2D image manipulation (video, images, textures).
- **CHOPs (Channel Operators):** Work with numeric and time-sliced data (motion capture, audio, LFOs).
- **SOPs (Surface Operators):** Handle 3D geometry.
- **DATs (Data Operators):** Deal with tables, scripts, and text-based data.
- **MATs (Material Operators):** Apply materials and shaders to 3D geometry.

### 2. Networks and Nodes
In TouchDesigner, you build projects by connecting nodes in networks. Each node represents a single operator that performs a specific task. Networks are hierarchical, meaning that you can create **sub-networks** inside each node to keep projects organized.

### 3. Parameters and Expressions
Each operator comes with its set of parameters, which can be customized. You can also link parameters using **expressions** (often written in Python) to dynamically control how your project behaves.

For example, you might use a simple Python expression to link the rotation of one object to the position of another:
```python
op('geo1').par.tx = op('geo2').par.ty * 2
```

### 4. DAT-to-OP Communication
DATs (tables or text data) can communicate with other OPs through scripting. This is especially useful when working with interactive setups or external data inputs like APIs or databases.

## Building a Basic Network

### Step 1: Creating a Base Network

- **Start by creating a `Constant CHOP`**: This generates a static channel of data. You can use this as a simple controller for other components. 
  - Example: Create a slider to control the brightness of a `Constant TOP`.

- **Add a `Constant TOP`**: This can represent an image plane with a constant color.
  - Go to the parameters panel and adjust the `RGBA` values to change the color of the texture.

- **Connect the `Constant CHOP` to the `Constant TOP`**: You can link the channel output of the CHOP to control the parameters of the TOP. For example, you can drive the `brightness` or `opacity` by referencing the CHOP value.
  - Use the expression: `op('constant1')['chan1']` to pull the value from the `Constant CHOP`.

### Step 2: Adding Interaction

- **Add a `Mouse In CHOP`**: This will allow you to use the position of the mouse as input data.
  - The `tx` and `ty` channels from the `Mouse In CHOP` provide normalized values for horizontal and vertical position.

- **Link the `Mouse In CHOP` to the parameters**: You can control the position of an object by linking the output of the `Mouse In CHOP` to a `Transform TOP`. Use expressions to dynamically set the `translate x` and `translate y` values:
  
  ```python
  op('transform1').par.tx = op('mousein1')['tx']
  op('transform1').par.ty = op('mousein1')['ty']
  ```

### Step 3: Applying Shaders (MATs)

- **Create a `Phong MAT`**: This is a basic material shader that you can apply to 3D geometry. Adjust its parameters to change the appearance of your 3D objects.
  
- **Apply the `Phong MAT` to a `Sphere SOP`**: After creating the `Sphere SOP`, drag the `Phong MAT` onto the `Sphere SOP`. The shader will now define how the object interacts with lighting in the scene.

- **Use a `Light COMP` and `Camera COMP`**: These components will allow you to properly illuminate and view your 3D geometry. Add them to your network and adjust their position and rotation for optimal viewing.

## Data-Driven Visualization

TouchDesigner shines in real-time data visualization. Here's how to create a simple audio-reactive visual.

### Step 1: Importing Audio

- **Add an `Audio File In CHOP`**: Import an audio file and output its wave data as channels. You can connect this CHOP to a `Math CHOP` to normalize or scale the values.

### Step 2: Controlling Visuals with Audio Data

- **Link Audio to Visual Parameters**: You can link audio amplitude to parameters like scale or color. For instance, drive the scale of a geometry based on the volume of the audio:
  
  ```python
  op('geo1').par.sx = op('audiofilein1')['chan1']
  op('geo1').par.sy = op('audiofilein1')['chan1']
  ```

### Step 3: Advanced Visualization Techniques

- **Add a `Trail SOP`**: Combine this with audio-reactive geometry to create a trailing effect that responds to music.

- **Use `Feedback TOP`**: For more complex effects, a `Feedback TOP` can be used to create a recursive visual feedback loop, adding depth to your visualization.

## Python Scripting and Customization

TouchDesigner allows deep customization using Python scripting. You can use scripts to automate behaviors, trigger events, or create complex interactions. For example:

- **Button Control**: Use a button to trigger specific actions.
  
  ```python
  def onClick():
      op('geo1').par.tx = 0
  ```

- **OSC/MIDI Integration**: Python can be used to process incoming OSC or MIDI signals and map them to visual parameters.

## Performance Optimization

- **Use the Performance Monitor**: To ensure smooth playback, especially for live visuals, always monitor performance using the built-in `Performance Monitor`. Optimize by reducing the number of unnecessary nodes, and using the `Cache TOP` for pre-rendered content when needed.

- **Adjust Resolution**: You can save processing power by adjusting the resolution of TOPs when working with 2D content. Lowering texture resolution can significantly improve performance.

## Final Tips

- **Hierarchies and COMPs**: Use `Container COMPs` and other components to group related nodes and make your network easier to manage.
- **Learning Resources**: For advanced techniques, always refer to the [Derivative documentation](https://docs.derivative.ca) and community tutorials.
```