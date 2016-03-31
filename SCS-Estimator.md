StandardGUIActions - gui constructed

All 
DRCKinematicsBasedStateEstimator createStateEstimator

- WrenchBased foot switch used  ... stateEstimatorParameters.getFootSwitchType()

8 contact detectors running:
- two per class
- two per foot
- two types: control and state estimator
ValkyrieStateEstimatorParameters ... contains all the magic numbers


class DRCEstimatorThread() 
 DRCEstimatorThread::createStateEstimator()
 creates a foot switch (for each foot). creates a foot switch in footSwitchesForEstimator
 returns class DRCKinematicsBasedStateEstimator()

class DRCKinematicsBasedStateEstimator()
 contains:
 pelvisLinearStateUpdater() ... estimates actual linear state
 PelvisRotationalStateUpdater()
 forceSensorStateUpdater()
 jointStateUpdater()
 imuOrientationBiasEstimator()


class: pelvisLinearStateUpdater()
 contains:
 PelvisKinematicsBasedLinearStateCalculator()
 PelvisIMUBasedLinearStateCalculator()
 ... two estimators and there is a dial between them:

DRCEstimatorThread::run()
calls: DRCKinematicsBasedStateEstimator::doControl()
 calls: pelvisLinearStateUpdater::updateRootJointPositionAndLinearVelocity
  calls: <>::computeLinearStateFromMergingMeasurements
   calls: <>::computeLinearVelocityFromMergingMeasurements and <>::computePositionFromMergingMeasurements
    carries out weighted combination of two sources of linear pos/vel measurements








      yoRootJointPosition.getFrameTuple(rootJointPosition);
      imuBasedLinearStateCalculator.updatePelvisPosition(rootJointPosition, pelvisPositionIMUPart);
      kinematicsBasedLinearStateCalculator.getPelvisPosition(pelvisPositionKinPart);

      pelvisPositionIMUPart.scale(alphaIMUAgainstKinematicsForPosition.getDoubleValue());
      pelvisPositionKinPart.scale(1.0 - alphaIMUAgainstKinematicsForPosition.getDoubleValue());

      rootJointPosition.set(pelvisPositionIMUPart);
      rootJointPosition.add(pelvisPositionKinPart);
      yoRootJointPosition.set(rootJointPosition);