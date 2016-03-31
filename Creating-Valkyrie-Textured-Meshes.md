Marco provided obj, stl could be bad due to polygons and not triangles


Gazebo:
- dae displays both color and mesh well
 gazebo won't read obj ... gives errors
- stl file are broken shards

Rvis:
 - dae displays both color and mesh well
 - obj displays mesh well in rvis 
 - stl files are broken shards in rvis too
 
Drake-Designer:
- stl read just fine in drake-designer (i think?)
- dae not read by vtk?
- is vtp preferred?
- designer might be case sensitive?

Atlas:
- obj is the used in MIT's urdf
- dae is default type from OSRF for Atlas:
https://bitbucket.org/osrf/drcsim/src/b16fe373ec7aff462024b076dfb641f9447a9b3f/atlas_description/urdf/atlas_v5.urdf?at=default

provided to user: dae
our conversion: dae -> obj -> vtp


Won't load:

        <mesh filename="package://val_description/meshes/head/Head_Multisense.dae"/>

Fine (without the texture) - might have read the obj instead:

    <mesh filename="package://val_description/meshes/head/head_multisense.dae"/>

  Add this to launch gazebo file to pause at start:
      <arg name="paused" value="true"/>

Need to remove this tag for gazebo to use textured head

    <gazebo reference="Head">
    <material>Gazebo/White</material>
  </gazebo>