# <img src="https://static.sidefx.com/images/apple-touch-icon.png" width="25" height="25" alt="Hbuild Logo"> Simshot 1.0

![license](https://img.shields.io/badge/license-MIT-green) ![version](https://img.shields.io/badge/version-1.0-blue) 

### <img src="https://github.com/SpongeFX/simshot/assets/152398516/31636e27-4844-458d-ab9b-709a38ae4b06" width="35" height="35" align="center" alt="Simshot Logo" >  Simshot is a Houdini SideFX digital asset for accelerated development and customization of scenes with emulation of shots from different types of weapons, accompanied by various effects when hit.



https://github.com/SpongeFX/simshot/assets/152398516/8f4dfd2c-98bf-4934-b3d4-8d5e296b0c8d



Simshot moves points along specified trajectories, determining collisions with polygonal geometric objects, where it creates points at the intersections of polygons and a polygonal model of the trajectory of movement inside the polygonal object. Asset calculates the rebound effect with a configurable bounce angle threshold. The points and polygons created as a result of calculations have special attributes for applying accompanying effects.


## 💾  Installation 
[Download the HDA file](otls/simshot.1.0.hdalc) and install it to your `houdini19.5/otls/` folder. For detailed instructions, please refer to the [Houdini documentation](https://www.sidefx.com/docs/houdini/assets/install.html)

## ☑️ Features
+ Tested in Houdini versions: 19.5
+ SOP/DOP packed and unpacked geometry support.
+ Interactions with simulation and/or animation in real time.
+ Rebound Effect: Customizable bounce when hitting polygons at sharp angles.
+ Explosion mode for placement at the locations of the sources of explosion in collisions.
+ Flexible velocity settings for use in simulations.
+ Defines polygons that occur inside other polygon objects in RBD simulations with basic collision parameter values, which can significantly reduce simulation time in scenes where other effects do not require high collisions.
+ The emitter of points can move during simulation.

## 🏃 Quick guide how to use the Simshot

<table>
    <tr>
        <td><b>1</b>. Go to the <b>Source</b> tab, select the source and the type of incoming geometry in Geometry Source.</td>
        <td><img src="https://github.com/SpongeFX/simshot/assets/152398516/b2e55d0a-c313-4cb8-b9d6-221043504d2e" width="191" height="63"></td>
    </tr>
    <tr>
        <td><b>2</b>. Specify the path to the geometry or connect to <i>input3</i> depending on the <b>Geometry Source</b> selection.</td>
        <td><img src="https://github.com/SpongeFX/simshot/assets/152398516/1c6c13e2-02a0-4066-b2ad-0267682b1c0d" width="191" height="63"></td>
    </tr>
    <tr>
        <td><b>3</b>. If you selected DOP as your geometry source, set the objects to import in the Objects section.</td>
        <td><img src="https://github.com/SpongeFX/simshot/assets/152398516/c65aaaa8-54c3-4c80-8289-6ad06b540fc6" width="191" height="63"></td>
    </tr>
</table>

#### Requirements for incoming geometry:
- Incoming geometry must have a string point attribute with a unique value for each object.
- If you are using fragmented geometry, there should be a group attribute specifying whether each primitive is inside or outside.
- Optional: Vector point attribute named "<i>velocity(v)</i>" is necessary if <a href="#enable_opo"><i>Output Point Offset</i></a> is enabled.
- For unpacked geometry to work correctly, each primitive in the incoming geometry must have a primitive attribute called "<i>xform</i>" containing a matrix of transformations.

<b>4</b>. Go to the <b>Setup</b> tab, and set name of the attribute with a unique value <img src="https://github.com/SpongeFX/simshot/assets/152398516/e4af500d-5b88-4cec-83c0-755b12f8f35b" width="170" height="23" align="center" alt="pieceattrib" > for each object, if it is different from the default name. If you are using fragmented geometry with closely spaced primitives, enable <a href="#fragmented">Fragmented Geometry</a>. Read the description of the <a href="#parm">parameters</a> and configure it as you want.

<b>5</b>. Add emitter and target points. 
To conveniently add points for calculating the trajectory of movement, it is recommended to use [HDA aim](https://github.com/SpongeFX/aim/).
1. If you are using HDA aim and have disabled "<i>direction correction</i>", and then go <a href="#next_0">next step</a>.
2. If "<i>direction correction</i>" is enabled, go to the ”<i>Targets</i>” tab and enable the "<i>Start Position Attribute</i>" toggle (recommend linking these parameters), and then go <a href="#next_0">next step</a>.

Alternative method without using HDA aim:  If you are not using HDA aim, you can set the start and target points with the necessary attributes in the “<i>Emitters</i>” and “<i>Targets</i>” tabs. However, please note that this method has limited functionality.

<b>Emitters</b> tab:
1. Enable the "<i>Enable Adding Points</i>" option.
2. Add a points and set the starting point positions.
   
<b>Targets</b> tab:
1. Turn on the "<i>Enable Adding Points</i>" option.
2. Change the next parameters if you want:<br>
<table border="0">
    <tr>
        <td valign="top" width="20%"><i>Start Frame</i></td>
        <td><p>Default start frame.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Frames Between Active</i></td>
        <td><p>The number of inactive frames between the creation frames of target points.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Speed</i></td>
        <td><p>The parameter value used as the point movement speed.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Frames Between Active</i></td>
        <td><p>Default emitter number. This can be useful if you want to use multiple emitters. The emitter number is the number of the point imported into input1 or/and created in the *Emitters tab.</p></td>
    </tr>
</table>  

3. Add a points and set the target point positions.
   
<a id="next_0"></a><b>6</b>. Set the settings in the <b>Explode</b> tab:
1. Add a moving point to a special group temporarily during the collision.
2. Optionally, remove this point from the simulation later.
3. This can be useful as a reference position for the source of the explosion.
4. Review the settings in the <a href="#explode"><i>Explode</i></a> tab if you are interested in this functionality.

<b>7</b>. Set the settings for bounces from primitives when intersecting:
<table>
    <tr>
        <td valign="top">In the <a href="#ric">Ricocheted</a> tab, you have the option to disable or enable the bounce effect when colliding with sharp angles on primitives. You can also adjust the bounce angle threshold and other parameters.</td>
        <td width = "252"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/d7417ee7-d1c2-4656-b288-ff28a3101e3b" width="248" height="101"></p></td>
    </tr>
</table><br>

<b>8</b>. Once you have created and positioned the emitters and target points, activate the timeline to run the asset simulation. If you are using HDA aim, you can create target points during the simulation while the timeline is active.

<b>9</b>. Cache the result you like and apply the effects you need using the points, primitives and their attributes produced by the asset. To view the points and primitives generated by an asset, add a Merge node, set its display flag, and connect it to the asset's *output1* and *output2*.<br>

### Table of attributes and groups generated by the asset

<table border="0">
    <tr>
        <td colspan="2" align="center"><b><i>Attributes</i></b></td>
    </tr>
    <tr>
        <td valign="top" width="18%"><i>i@chk</i></td>
        <td><p>Is a state attribute. Its value is &quot;1&quot; when the moving point is inside some geometric object in the current frame, &quot;0&quot; - outside the objects, &quot;8&quot; if &quot;<i>Explode Mode</i>&quot; is enabled and intersection of primitives is defined.</p></td>
    </tr>
        <tr>
        <td valign="top"><i>f@cur_frame</i></td>
        <td><p>Is the creation frame. It has a value of 0 for moving points.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>v@dir</i></td>
        <td><p>Is the trajectory for moving points (<a href="#speed"><i>f@speed</i></a> > 0.0001). It is assigned to points created as an asset and belonging to the &quot;<i>enter</i>&quot; group as a <i>velocity</i> multiplier for applying effects. See description the parameter <a href="#spole">Spole_mult</a> to understand how it works in practice.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>f@dlim</i></td>
        <td><p>Is the distance that a point can come inside an object before stopping. See description the parameter <a href="#dlim">Use Target Points Attribute</a> to understand how it works in practice.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>i@hact</i></td>
        <td><p>The attribute has a value of &quot;1&quot; in the frame of intersection and &quot;0&quot; when no intersections are defined. It is used as an emitter to apply <i>velocity</i> patterns.</p></td>
    </tr>    
    <tr>
        <td valign="top"><i>i@id</i></td>
        <td><p>All moving points (<i>f@speed</i> > 0.0001) have a unique attribute value <i>id</i> equal to the point number. All created points receive the <i>id</i> of the point that initiated their creation.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>i@inter</i></td>
        <td><p>For moving points this attribute has the value of the last intersected primitive number. For points made in asset, this attribute has the value of the primitive number from which the transformation matrix and <i>@N</i> are imported.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>v@N</i></td>
        <td><p>The normal of the primitive with which the intersection was defined for points made in asset, used to adjust the velocity vector when applying effects.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>s@name</i></td>
        <td><p>For moving points, it has the value of the unique identifier of the last intersected object.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>i@ncid and i@nid</i></td>
        <td><p>Intersection identifiers attached to the object. All points created on a certain intersected object have a unique attribute added, which has the same value for all points associated with this object. This is used to create a unique attribute value for building a primitive curve that repeats the trajectory inside the object. It is also used to link effects to specific objects. The value of <i>@ncid</i> for a moving point represents the number of intersected objects.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>s@name_trail</i></td>
        <td><p>The identifier for linking points and primitives to a specific object.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>v@odir</i></td>
        <td><p>Created during bounces in the bounce <i>@Frame</i>. It has the value of the trajectory along which the point moved before bouncing. Used for applying effects.</p></td>
    </tr>
    <tr>
        <td valign="top"><a id="speed"></a><i>f@speed</i></td>
        <td><p>The point movement speed.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>v@spos</i></td>
        <td><p>Position for applying the transformation matrix.</p></td>
    </tr>
        <tr>
        <td valign="top"><i>s[]@stack</i></td>
        <td><p>Identifiers of the last intersected objects. Only for moving points.</p></td>
    </tr>
    <tr>
        <td colspan="2" align="center"><b><i>Groups</i></b></td>       
    </tr>
    <tr>
        <td valign="top"><i>entry</i></td>
        <td><p>The group of points created at the position of the first intersection with an object.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>exit</i></td>
        <td><p>The group of points created at the position of the last intersection with an object.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>moved</i></td>
        <td><p>The transformation matrix is applied to the points in this group. Points are added to this group after a temporal delay in the parameter <a href="#delay"><i>Transform Matrix Delay</i></a>.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>moving</i></td>
        <td><p>The group for moving points</i></a>.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>ob_in</i></td>
        <td><p>Identical to the entry group, but the points remain in the group until the end of the simulation</i></a>.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>ob_out</i></td>
        <td><p>identical to the exit group, but the points remain in the group until the end of the simulation</i></a>.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>ric_pt</i></td>
        <td><p>Points created at the position of a bounce are added to this group</i></a>.</p></td>
    </tr>
</table><br>

If you import geometry from a DOP simulation, you can send the *velocity*(v) back to the source of the imported simulation for reverse interaction. This feature is not recursive, but it allows you to interact with the simulation in real time if your computer is powerful enough.

<br>


## 🔀 Description of Inputs and Outputs:

<table border="0">
    <tr>
        <td width="17%" td rowspan="2" valign="top">📥 Inputs:</td>
        <td><p><i>Input1</i> - This input can be used to connect emitter points to the asset. It can consist of one or more points with either a changing or static position. The path to the “<i>Emitter points</i>” can also be set in the “<i>Source</i>” tab.</p></td>
    </tr>
    <tr>
        <td><p><i>Input2</i> - This input can be used to connect target points to the asset. It can consist of one or more points with the necessary attributes for the asset to function correctly. The path to the “<i>Target points</i>” can also be set in the “<i>Source</i>” tab.</p></td>
    </tr>
    <tr><td td rowspan="5" valign="top">📤 Outputs:</td>
        <td><p><i>outpout1</i> - This output consists of moving points and points created by the asset when collisions and bounces are detected.</p></td>
    </tr>
    <tr>
        <td><p><i>outpout2</i> - This output consists of points and polygons that follow the trajectory of movement within polygonal geometric objects. The parameters for this output can be configured in the <a href="#trails">Trails</a> tab.</p></td>
    </tr>
    <tr>
        <td><p><i>outpout3</i> - This output contains points with the <i>velocity(v)</i> attribute, which can be used for interaction with other simulations. The parameters for this output can be configured in the <a href="#vel">Velocity</a> tab.</p></td>
    </tr>
    <tr>
        <td><p><i>outpout4</i> - This output consists of unpacked incoming geometry (if packed geometry was submitted) without the points that were created by the asset. The primitive attributes <i>@N</i> and <i>@xform</i> are added.<br>
        <u>Important feature</u>: If "<i>Fragmented Geometry</i>" is enabled, all objects(and pieces) will be  scaled down by the value of the "<i>Pieces Scaling</i>" parameter. This may affect the final image if you plan to use this data in the render.</p></td>
    </tr>
        <tr>
        <td><p><i>outpout5</i> - This output is used to visualize emitter and target points while creating and configuring positions in the “<i>Emitters</i>” and “<i>Targets</i>” tabs.</p></td>
    </tr>
</table><br>
<a id="parm"></a>
 
## 📂 Tabs and Parameters

### 📁 Source
The incoming target points must have the following point attributes: <i>i@emit</i>, which represents the emitter point number, and <i>f@speed</i>, indicating the speed of the point along its trajectory. If the "<i>Start Position Attribute</i>" is enabled in the <i>Targets</i> tab, an additional point attribute <i>v@startpos</i> needs to be added. The point movement will begin once the value of the attribute <i>f@speed</i> exceeds 0.

To select the source and type of incoming geometry go to the "<i>Geometry Source</i>", and set the path to the geometry.

<table border="1">
    <tr>
        <td valign="top" width="18%"><i>Transfer Attributes</i></td>
        <td><p>If you choose DOP as the geometry source, you can use the "<i>Transfer Attributes</i>" parameter to transfer specified attributes to the unpacked geometry, which will be available in <i>output4</i>.</p></td>
    </tr>
</table><br><a id="pram"></a>

### 📁 Setup

In the “Setup tab”, you can find settings that affect the search and detection of collisions during the simulation.

<table border="1">
    <tr>
        <td valign="top" width="18%"><i>Pieces attributes</i></td>
        <td colspan="2"><p>The name of the point attribute containing the unique identifier.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Object Size</i></td>
        <td colspan="2"><p>This parameter controls the distance at which intersections are searched for in cases involving moving objects during collisions. It is used to determine whether the moving point is still inside or outside the object by performing an additional check. It is recommended to set this parameter to the maximum thickness of the largest object that might be affected by the simulation.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Threshold</i></td>
        <td colspan="2"><p>The threshold parameter is used to shift the position in the direction of the intersection search. This helps to avoid falsely detecting intersections with the geometry of the search source.</p></td>
    </tr>
    <tr>
        <td valign="top"><a id="delay"></a><i>Transform Matrix Delay</i>
    </td>
        <td valign="top">The transform matrix delay parameter determines the delay in applying the transformation matrix to points created by the asset when intersections are detected. Increasing this parameter can be useful when creating effects, such as a laser beam passing through a primitive for more than one frame. The value is specified in frames.</td>
        <td width="36%"><img src="https://github.com/SpongeFX/simshot/assets/152398516/ccb41b7d-acce-4225-a12b-9b6cc7695601"></td>
    </tr>
    <tr>
        <td valign="top"><i>Group Removal Delay</i></td>
        <td colspan="2"><p>Delay in removing points created by an asset from the entry and exit groups creates a delay in the removal of points. The value is given in frames. Points are added to these groups in the creation frame, which allows for the application of effects at the moment of collision.</p></td>
    </tr>
        <tr>
            <td valign="top"><a id="fragmented"></a><i>Fragmented Geometry</i></td>
            <td colspan="2"><p>Enabling this parameter allows for efficient processing of fragmented geometry when using RBD solvers with low collision settings. It redefines the order of intersected primitives in the case where one geometric object has penetrated another. Ricochet only works on polygons in the <i>outside</i> group.</p></td>
        </tr>
        <tr>
            <td valign="top"><i>Pieces Scaling</i></td>
            <td colspan="2"><p>The reduction coefficient of objects increases the accuracy of detecting collisions with closely spaced primitives that have the same position.</p></td>
        </tr>
        <tr>
            <td valign="top"><a id="onhit"></a><i>On-Hit attribute</i></td>
            <td colspan="2"><p>Enabling adds a point attribute called <i>i@hact</i> to moving points. This attribute has a value of 1 when an intersection has occurred, and 0 when there are no intersections. It helps in applying <i>velocity(v)</i> only at the moment of collision and movement inside the geometric object. This attribute can also be used as a collision indicator. Disabling this attribute makes the application of <i>velocity(v)</i> constant, which can cause the use of force on objects that did not intersect but had sufficient distance. Sometimes this effect can look interesting, like a shockwave from a projectile flying near an object, but it is often an undesirable effect.</p></td>
        </tr>
            <tr>
                <td valign="top"><a id="enable_opo"></a><i>Enable Output Point Offset</i></td>
                <td colspan="2">This parameter activates the search for an alternative trajectory in situations where the first intersection found should create an exit point on an object with which there were no previous intersections. The search is performed taking into account the object's <i>velocity(v)</i>, and involves searching for curved surfaces, calculating the trajectory, and creating additional points.<br><br><img src="https://github.com/SpongeFX/simshot/assets/152398516/6a561b00-c465-4e65-9fbf-12308bc312ee" width="45%"> <img src="https://github.com/SpongeFX/simshot/assets/152398516/d22a18d5-f4bd-479f-ba5e-ccc4f994c8c6" width="45%"></td>
            </tr>
            <tr>
                <td valign="top"><i>Search distance(%)</i></td>
                <td colspan="2"><p>The minimum distance to search for intersections.</p></td>
            </tr>
            <tr>
                <td valign="top"><i>Output Point Position Offset</i></td>
                <td valign="top">The offset of the position of the output point in the direction of movement of the collision object.<br></td>
                <td><img src="https://github.com/SpongeFX/simshot/assets/152398516/d10017d9-2b6a-4ce6-9615-52ea2143c6e2"></td>
            </tr>
            <tr>
                <td valign="top"><i>Stop Points</i></td>
                <td colspan="2"><p>Enables the stop of a moving point inside the object if the distance specified in the “<i>Distance to stop</i>” parameter has been passed inside the object. By default, the toggle is turned off and if the <a href="#expmode"><i>"Enable Explode Mode"</i></a> is not activated, then the point moves along the trajectory nonstop. The distances of all passed objects are summed up for each moving point individually.</p></td>
            </tr>
            <tr>
                <td valign="top"><i>Use Target Points Attribute</i></td>
                <td colspan="2"><p id="dlim">Enables checking target points for the presence of a point float attribute called <i>f@dlim</i>. If the attribute is present in the incoming points, its value will be used to calculate the stopping distance. This allows for manual control of the distance that a point inside the object must travel for each point individually. The dynamically changing values of <i>f@dlim</i> can be seen during simulation in the Geometry Spreadsheets when intersections occur. The covered distance is summed up and subtracted from the value of the attribute <i>f@dlim.</i></td>
            </tr>
            <tr>
                <td valign="top"><i>Distance To Stop</i></td>
                <td colspan="2"><p>If incoming target points do not have <i>f@dlim</i> and &quot;<i>Use Target Points Attribute</i>&quot; is disabled, then the value of <i>f@dlim</i> will be set for each point, as specified in this parameter.</p></td>
            </tr>
            <tr>
                <td valign="top"><i>Random</i></td>
                <td colspan="2"><p>Adds random values limited by the min max parameters to the value of attribute <i>f@dlim.</i></p></td>
            </tr>
            <tr>
                <td valign="top"><i>Min</i></td><td colspan="2"><p>Minimum deviation value of randomization.</p></td>
            </tr>
            <tr>
                <td ><i>Max</i></td>
                <td colspan="2"><p>Maximum deviation value of randomization.</p></td>
            </tr>
</table><br>
<a id="explode"></a> 

## 📁 Explode
In this tab, you can configure the behavior of points in a collision, making it convenient to attach a [*PyroBurstSource*](https://www.sidefx.com/docs/houdini/nodes/sop/pyroburstsource.html) or something similar. Refer to the [example](#sim_explode).

<table>
    <tr>
        <td width="56%"><p><video src='https://github.com/SpongeFX/simshot/assets/152398516/13dda40a-d715-41f9-a1dd-3462b000db3d'></video></p></td>
        <td width="43%"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/3e653ccb-0f8d-4462-91df-d21c132bda6b"></p></td>
    </tr>
</table>

<table border="1">
    <tr>
        <td valign="top" width="18%"><a id="expmode"></a><i>Enable Explode Mode</i></td>
        <td><p>When a moving point detects a collision with a primitive, the point is added to the group for a certain number of frames and then removed from the simulation.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Group</i></td>
        <td><p>The name of the group to which the points will be added when the intersection is defined.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Distance to explosion</i></td>
        <td><p>The distance that the point will move from the intersection position in the direction of the trajectory before being added to the group.</p></td>
    </tr>
    <tr>
        <td valign="top"><a id="edelay"></a><i>Explode Delay</i></td>
        <td><p>Delay in adding to the group. Value is in frames.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Explode Duration</i></td>
        <td><p>The duration of the point's presence in the group. After the specified value expires, the point is removed from the simulation. Value is in frames.</p></td>
    </tr>
</table><br>
<a id="ric"></a> 

## 📁 Ricochete
In this tab, you can enable or disable the bouncing effect, adjust the threshold of the angles at which a bounce should occur from surfaces, and adjust the bounce trajectory.

<table>
    <tr>
        <td width="50%"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/8571a3ec-203f-4407-94f0-3f6950d121c4"></p></td>
        <td width="50%"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/61ceab30-7bf2-4051-b5dc-cd1948c6a6bd"></p></td>
    </tr>
</table>
<table border="1">
    <tr>
        <td valign="top"  width="18%"><i>Enable Ricochet</i></td>
        <td colspan="2"><p>This toggle will activate the checking of the angle of bounce from the polygon, followed by changing the trajectory when the bounce conditions are met, and creating points added to the &quot;ric_pt&quot; group at the bounce locations from polygons.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Rebound Angle Threshold</i></td>
        <td valign="top" ><p>The angle between the polygon normal and the trajectory of the point. If the obtained value is greater than the parameter value, then a new trajectory and a new position are calculated, where the point will be moved and the trajectory attribute will be changed. The parameter value is in the range from 90 to 180 degrees.</p></td>
        <td width="28%"><img src="https://github.com/SpongeFX/simshot/assets/152398516/82221dc5-544f-4316-b718-24a9625170e7"></td>
    </tr>
    <tr>
        <td valign="top"><i>Rebound Angle Min</i></td>
        <td colspan="2"><p>Minimum deviation from the bounce trajectory. When performing a bounce and calculating a new trajectory, a random function is used with remapped output values determined by this parameter.</p></td>
    </tr>
    <tr>
        <td valign="top"><ip>Rebound Angle Max</i></td>
        <td colspan="2"><p>Maximum deviation from the bounce trajectory. When performing a bounce and calculating a new trajectory, a random function is used with remapped output values determined by this parameter.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Group Removal Delay</i></td>
        <td colspan="2"><p>Delay in deleting points created during the bounce from the &quot;ric_pt&quot; group. Value is in frames.</p></td>
    </tr>
    <tr>
        <td rowspan="2" valign="top"><p>Enable attributes Without Bounce</p></td>
        <td colspan="2">Enable the attribute to exclude bouncing. All intersected primitives will be checked and if a match is found with the attribute name and value, the bounce calculation will not be performed. This can be useful if there are objects in the intersecting geometry that should not cause a ricochet effect, such as a sheet of paper or cloth.</td>
    </tr>
    <tr>
        <td colspan="2"><img src="https://github.com/SpongeFX/simshot/assets/152398516/c29050fa-c2e2-489a-adf3-479166288cda" width="49%">
        <img src="https://github.com/SpongeFX/simshot/assets/152398516/9d83a854-a68c-4408-8f99-a8f778c361e6" width="49%"></td>
    </tr>
        <tr><td><p>Attribute Name</p>
</td><td colspan="2"><p>The name of the attribute to exclude bouncing.</p>
</td>
</tr><tr><td><p>Attribute Value</p>
</td><td colspan="2"><p>The value of the attribute to exclude bouncing.</p>
</td>
</tr>
</table>

If the source of geometry for intersections is fragmented geometry and the "*Fragmented Geometry*" toggle is disabled, then the bounce calculation will also be performed for polygons in the *inside* group.

<table>
    <tr>
        <td width="48%"><p>Fragmented Geometry enabled. <img src="https://github.com/SpongeFX/simshot/assets/152398516/1c6b08d9-eebf-4b6d-84bf-7533805f4e5f"></p></td>
        <td width="48%"><p>Fragmented Geometry disabled. <img src="https://github.com/SpongeFX/simshot/assets/152398516/fc6e3034-c61b-46f0-8e75-096bdd1e7f86"></p></td>
    </tr>
</table><br>
<a id="trails"></a> 

## 📁 Trails 	 
In this tab, you can configure primitive curves that follow the trajectory of movement inside the object. The points for the subsequent connection of their polygons are created in the POP DOP Network inside the asset and the settings in the *Source*, *Birth*, *Attributes* tabs are the POP Source settings inside popnet. The emitter for creating points are the entry and exit points, as well as a moving point while it is inside the object. Each curve related to a specific object has an attribute with a unique value that allows it to be identified separately or as a group by the id of the point that created the line. The attribute value is of type "0_1", where "0" is the id of the point that determined the intersection, and "1" is the id of a specific line belonging to one object. 

<table border="1">
    <tr>
        <td valign="top" width="18%"><i>Attribute Name</i></td>
        <td><p>The name of the attribute with the curve ID.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Transform Matrix Delay</i></td>
        <td><p>Delay in applying the transformation matrix to points. Increasing the parameter can be useful if you want to create, for example, the effect of a laser beam that passes through the primitive for more than 1 frame. The value is in frames.</p></td>
    </tr>
</table><br>

<table>
    <tr>
        <td width="48%"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/461da5b9-e77a-4aff-8309-ad1faa70bd2b"></p></td>
        <td width="48%"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/7d6569ba-198e-42cf-ac02-6a76054c9614"></p></td>
    </tr>
</table>

In the *Smooth* tab, curve smoothing settings identical to the ”*Smooth*“ SOP node are available.

<br><a id="vel"></a>

## 📁 Velocity
In this tab, you can create and configure a vector point attribute of *velocity*(v) for use in various scenarios. For example, you can transfer velocity to the rbd simulation in order to influence it during the simulation. It works like this: at the moment of collision or constantly (depending on the state of the [*On-Hit*](#onhit) toggle), a line is created from points with the *velocity*(v) attribute, and then this data is imported into the simulation. The *velocity*(v) here is the normalized vector of the direction "*v@dir*" of motion multiplied by the speed attribute "*f@speed*". To see the template, turn on the “*Velocity Template Visualization*” toggle, create and connect Null to *output3*, and turn on the null display flag. Enable displaying point trails in the viewport to visualize the *velocity*(v). Customize the template according to your preferences and turn off the “*Velocity Template Visualization*” toggle.

<table>
    <tr>
        <td width="52%"><video src="https://github.com/SpongeFX/simshot/assets/152398516/3566d93a-2ded-4148-a4db-d76a52496a00"></video></td>
        <td width="35%"><img src="https://github.com/SpongeFX/simshot/assets/152398516/63e8ba11-251f-4d83-96e9-a1759503f295"><img src="https://github.com/SpongeFX/simshot/assets/152398516/2fa138fa-50f9-41c1-8589-71776fcaf320"></td>
    </tr>
</table>
<br>
<table border="1">
    <tr>
        <td valign="top"  width="18%"><i>Velocity Template Visualization</i></td>
        <td valign="top"><p>Enable the template display to adjust the direction of the velocity vectors.</p></td>
        <td width="35%"><img src="https://github.com/SpongeFX/simshot/assets/152398516/5da4867d-9641-4657-8365-6c8618bc7627"></td>
    </tr>
    <tr>
        <td><i>Template Position</i></td>
        <td colspan="2"><p>The position of the template velocity visualization. By default, the template appears at the zero position, but you can move it to any convenient location for customization.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Template v Multiplier</i></td>
        <td colspan="2"><p>Vector multiplier for convenient visualization, depending on the scale (the values of this parameter only affect the visualization of the template). If you need to increase the strength in the simulation, use the “<i>Velocity Multiplier</i>” parameter.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Velocity Multiplayer</i></td>
        <td colspan="2"><p>The multiplier of the <i>velocity</i>(v).</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Number Of Points</i></td>
        <td colspan="2"><p>Number of points in a line.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Segment Length</i></td>
        <td colspan="2"><p>Distance between points.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Rotation Angle</i></td>
        <td colspan="2"><p>Rotation angle. The axis of rotation is the direction of movement.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Tilt Angle</i></td>
        <td colspan="2"><p>The angle of deviation from the direction of movement.</p></td>
    </tr>
    <tr>
        <td rowspan="2" valign="top"><i>Velocity In The Direction Of Rebound</i></td>
        <td colspan="2" valign="top">Change the direction of the <i>velocity</i>(v) during the rebound. By default, the <i>velocity</i>(v) in the rebound frame is directed in the direction of movement before the rebound. When the rebound is turned on in the frame, the <i>velocity</i>(v) will be directed towards the rebound. If you are using RBD simulation and you need more fragments to be carried away towards the rebound, enable this toggle.</p></td>
    </tr>
    <tr border="0">
        <td td colspan="2" ><p>Enable/Disable</p><img src="https://github.com/SpongeFX/simshot/assets/152398516/eed63159-01b4-4205-b3de-e76a38ed48f2" width="45%">
       <img src="https://github.com/SpongeFX/simshot/assets/152398516/550241fa-4e8f-426b-a89f-81bb7414a37c" width="45%">
        </td>
    </tr>
    <tr>
        <td valign="top"><i>Rebound Velocity Multiplier</i></td>
        <td colspan="2"><p>The multiplier of the rebound <i>velocity</i>(v). Replaces the “<i>Velocity Multiplayer</i>” value in the bounce frame.</p></td>
    </tr>
</table><br>

## 🔎 Example
The example file contains simshot and aim assets, as well as a few simple examples of interacting with simulations and applying effects. This example file does not include some parts of the scene that you can see in this article, such as the spark effect, DOP FLIP, and the chain of simultaneous interaction between RBD DOP/SOP, FLIP DOP, and the asset when the FLIP simulation takes into account collisions with packed geometry and affects to this geometry during the simulation. This has some configuration features, but the general principle is the same as in the example presented. It is also procedural and works in real-time, but slower because, as a rule, the FLIP simulation uses high-resolution VDB to calculate collisions with fragmented packed geometry.

Download the example file [*simshot_example.hiplc*](/example/simshot_example.hiplc) and the folder "*geo*" with the bullet geometry file inside. Download and install [HDA simshot](https://github.com/SpongeFX/simshot/otls/simshot.hdalc) and [HDA aim](https://github.com/SpongeFX/aim/otls/aim.1.0.hdalc). Open the example file, go to the Geometry object "*SIM_shooting*", select the HDA aim in the Network View, select the camera "*aim*" in the viewport, activate the timeline, and click the left mouse button (LMB) in the viewport window. You will see how the point on which the bullet geometry is copied will start moving along the trajectory from the camera position "*aim*" towards the position where the LMB click was made. When the trajectory intersects with the geometry, the force will affect the fragmented pieces.


https://github.com/SpongeFX/simshot/assets/152398516/22dc047a-f415-4ecd-9d7d-27a97f6f46df

<br>

### 📁 /obj/Boolean/
Go to the "*Boolean*" geometric object. Here you can add holes in the places where the intersections of the geometry were found, or disable it. The selection takes place in the *switch1* node. In the example file, the switch1 switch is already connected to the corresponding parameter of the geometry source in HDA simshot. If you select SOP and use unpacked geometry, then the corresponding input is selected in *switch1*, where unpacking geometry is excluded.
Changes to the geometry are made by the SOP node *Boolean*, to increase the calculation speed, only those objects with which the intersection was determined are selected.

If this is a packed geometry simulation, you can use the *velocity*(v) attribute in the simulation and leave holes at the same time. At the same time, if the values of the *velocity*(v) attribute are high and in the next frame the geometric object with which the intersection was recorded overtakes the moving point, then a short line of 0.001 length is created on the object, having a starting point at the position found in the last frame, added to the enter group, and an end point in the position - *@N* + 0.001, not added to the exit group. 

Geometry construction for *Boolean* is performed in SOP node polywire based on data from *output2* of HDA simshot, noise is added to *pointvop1*. 

You can also extend the proxy geometry of the lines so that they overlap the object for *Boolean* to work correctly.

Changing the length in the “*Value*” parameter in the Primitive Wrangle "*increase_the_length*". You can disable hole creation by toggle *switch2*. 

You can also disable unpacking for geometry not participating in the for-each loop.

<table>
    <tr>
        <td width="43%"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/eaafc214-b23d-4156-bcca-70674512f7ce"></p></td>
        <td width="56%"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/6662f664-d6c7-4209-9dae-9832fc19fd2c"></p></td>
    </tr>
</table><br>
<a id="sim_explode"></a> 

### 📁 /obj/SIM_explode/
The geometric object "*SIM_explode*" contains the emitter settings for the explosion. When Explosion mode is enabled in HDA Simshot, imported cars will be added to a special group in case of collision. Point positions are used for pyroburstsource. The points added to the group are assigned the attribute *f@cur_frame* to calculate the initial frame of the pyroburstsource and pyrosolver simulation. For correct calculation, it is recommended to connect the “[*Explode Delay*](#edelay)” parameter in "*start_frame_attrib*" with the parameter of the same name in HDA simshot, as shown in the example.
The Object Merge node "*OUT_V_EXPLODE*" import the velocity for interacting with the simulation. The interaction takes place through a velocity transfer inside the DOP Network "*dopnet_rbd*" using the Geometry VOP DOP "*velocity_explode*", which has 2 parameters: "*Force multiplier*" – a force multiplier that increases the impact, and "*Minimum Distance*" – the minimum distance from the points generated by pyroburstsource to the points of the simulation object to which the *velocity*(v) will be transferred.<br>
If you want to visualise smoke, activate the display flag on the "*RNDR_explode*" node.<br>
<br>

### 📁 /obj/SIM_smoke/
<table>
    <tr>
        <td width="50%"><p><video src="https://github.com/SpongeFX/simshot/assets/152398516/7a54ccfb-0a2c-42e1-85b3-0417649627a9"></video></p></td>
        <td width="50%"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/2376477a-f462-4b5f-8b6d-7974176714ef"></p></td>
    </tr>
</table>

Here, you can adjust the emission of smoke at collision sites or apply emitter settings for other effects. In the Network Box "*Emitters settings*", there are settings for the shape, the number of emitter points, and the vector attribute *velocity*(v). Select "*Display point trails*" in the viewport to see the *velocity*(v) vector. Set the display flag on the Attribute Wrangle node "*height_ctrl_ENTRY*" to see the geometry on which the emitter points will be applied. Set the height of the sphere slice (parameter "*Height*") and the angle of inclination of the *velocity*(v) vector (parameter "*Normal Angle*"), if needed. In the *scatter* node, set the desired number of emitter points and add noise in the Attribute VOP "*noise_vel*", if needed.

The Attribute Wrangle "*group_filter*" controls the duration of the emitters. The duration of emitter activity is set in the “*Group Removal Delay*” parameter, which in this example is associated with the “*Group Removal Delay*” parameter in HDA simshot, performing an identical function.

In Attribute Wrangle "*filter_for_nearby_sources*", points located in close positions are removed. This parameter is relevant for fragmented geometry when there is a collision with a fragmented object, whose pieces are tightly adjacent to each other. In this case, the emitter will only be created on the first and last intersected primitive. The emitter will also be created on intersected primitives if the distance between the intersected primitives is greater than the “*Threshold*” parameter. For example, when a fragmented object is being destroyed. This is necessary to avoid unnecessary calculations and emitters for effects inside the fragmented geometry in case it is not destroyed. Disable this in *switch1* if it is not required.

If you need smoke collisions, go to “*dopnet1*”, turn off bypass Volume Source Dop object “*collision_vol_src*", connect it via the *Merge* node and configure voxel size vdb in “*/obj/SRC_DOP_collision_geo/collisionsource*". This will increase the simulation time. You can also use *billowsmoke* from the toolshelf, sometimes it works faster.

If you want to enable smoke emission in intersections and visualise smoke, activate the display flag on the "*RNDR_smoke*" node.

<table border="1">
    <tr>
        <td width="65%" valign="top"><p id="spole">In Attribute Wrangle &quot;<i>spole_in_dir</i>&quot;, a vector attribute <i>v@dir</i> is added to the <i>velocity</i>(v) attribute assigned to the points in the enter and <i>ric_pt</i> group as a vector along the slope of the surface relative to the trajectory of the point. This changes the direction of the <i>velocity</i>(v), making it look more natural. For example, when sparks and fragments bounce off in the direction of the rebound trajectory or along the wall. You can control this effect in the &quot;<i>Spole_mult</i>&quot; parameter.</p></td>
        <td><img src="https://github.com/SpongeFX/simshot/assets/152398516/708ed0d5-099b-4098-9e40-444a9a446aa5"></td>
    </tr>
</table><br>

### 📁 /obj/SRC_SOP_collision_geo/
The source of the not simulated geometry contains 2 examples: a simple one, and an example with an attribute that excludes bounce, created in Attribute Wrangle “*add_atrib_rest_bounce*". Select your option using *switch1*.

<br>

### 📁/obj/SRC_DOP_collision_geo/
The source of the DOP geometry for working with the asset contains 3 examples:

FRAGMENTЕD WALL - simple fragmented geometry.<br>
RE-FRAGMENTED PIECES  - localized fragmented geometry for more detail.<br>
SAMPLE BUILDING - a simple building made of fragmented geometry for an explosion test.<br>
Select your option using *switch1*.


#### 📔 NetBox “DOP SIM”
This NetBox contains the DOP Network "*dopnet_rbd*", the simulation result of which is imported into HDA simshot to determine intersections. Go into "*dopnet_rbd*", inside you can see a standard set of nodes for working with packed geometry, such as rigidbody solver, gravity, constraint network, and others. There are also nodes with custom settings for interacting with the simulation. Points with the *velocity*(v) attribute are imported from HDA simshot to the Geometry VOP Dop "*velocity*", where the minimum distance from the imported points to the packed simulation object (the position of the center point) is determined. If the distance is valid, *velocity*(v) is transformed into *v@force* attribute, which is applied to the packed simulation object in the rigidbody solver. Settings allow you to flexibly adjust the effect of force on objects.

This NetBox contains the DOP Network "*dopnet_rbd*", the simulation result of which is imported into HDA simshot to determine intersections. Go into “*dopnet_rbd*”, inside you can see a standard set of nodes for working with packed geometry, such as rigidbody solver, gravity, constraint network and others. There are also nodes with custom settings for interacting with the simulation. Points with the attribute *velocity*(v) are imported from HDA simshot to the Geometry VOP Dop “*velocity*”, where the minimum distance from the imported points to the packed simulation object (the position of the center point) is determined. If the distance is valid, *velocity*(v) is transformed into v@force attribute, which is applied to the packed simulation object in rigidbody solver. Settings allow you to flexibly adjust the effect of force on objects.

#### 💡 VOP DOP “*velocity*” parameters:

<table border="1">
    <tr>
        <td width="42%" valign="top"><i>Force multiply</i></td>
        <td"><p>Power multiplier.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Minimum Value In Source Range</i></td>
        <td><p>The minimum distance to the packed geometric object.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Maximum Value In Source Range</i></td>
        <td><p>The maximum distance to the packed geometric object.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Minimum Value In Destination Range</i></td>
        <td><p>Remapping of the minimum distance value.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Maximum Value In Destination Range</i></td>
        <td><p>Remapping of the maximum distance value.</p></td>
    </tr>
</table>

#### 💡 Sopsolver:
Also in dopnet_rbd there is a sop solver in which constraints are removed during collisions. Go inside “*sopsolver*”. The Primitive Wrangle “*accel*” calculates acceleration (force divided by mass *v@force* / *f@mass*), and if the value exceeds the Threshold parameter value, the constraint is removed. Here you can add and configure segment clustering.

<table>
    <tr>
        <td colspan="2"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/f21eba08-1c27-4209-855b-27d531d56d97"></p></td>
    </tr>
    <tr>
        <td colspan="2"><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/897c054a-1ad1-4f7a-9986-a0db741a4cc1"></p></td>
    </tr>
</table>
<br>
<table>
    <tr>
        <td valign="top"  width="70%"><p>You can visualize the value of the force by coloring the packed geometry to which the force is transferred. Connect the unpack node to “OUT” and add Cd in the transfer attributes parameter.</p></td>
        <td><p><img src="https://github.com/SpongeFX/simshot/assets/152398516/f15ee57b-27b9-43b6-947b-08e78ad8fc53"></p></td>
    </tr>
</table>

🔸 For a stable simulation, make sure that the "*Collision Sourse*" node is located after the DOP Network and before the output.

<br>

### 📁 /obj/SRC_shell/
The settings of the geometry are copied to moving points. In Attribute Wrangle "*N_from_dir_and_add_vel*" node, you can adjust the velocity for motion blur. Disable the color in the "*color*" node if you need to. By default, the geometry is a light source, you can disable this in “*geolight_shell*”.

<br>

### 📁 /obj/SRC_scorch/
Here, a geometry is created that repeats the inner surface of the holes produced in *Boolean*("*SCORCH - Type-B*") or polywire, repeating the trajectories inside the object imported from *output2* HDA simshot ("*SCORCH - Type C*"). This geometry can be used to perform some effects, such as installing glow emitters (as shown in the example). To disable the glow effect on the geometry, turn off "*geolight_scorch*". Adjust the color and scale of the geometry in the nodes "*scale_and_color(SCORCH - Type B)*" and "*color_and_width(SCORCH - Type C)*", and adjust the noise in "*add_width_noise*" if needed. To turn on scorch, enable the light in the geometry light node "*geolight_scorch*".




https://github.com/SpongeFX/simshot/assets/152398516/f8049cf8-76f5-48cb-accd-8200da74cc7e




## 💬 Feedback

This version does not reduce the speed after collisions and bounces, and also does not allow you to determine the density and type of materials under various processing scenarios, and much more. I have a lot of ideas on how to develop the asset and this direction, but maybe someone will have interesting suggestions or suggestions, it would be very interesting to know about it, and If you have any feedback or run into issues, please feel free to open an issue on this GitHub project.
