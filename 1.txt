import time
import json
import random
import rclpy
from std_msgs.msg import String
from rclpy.node import Node

class PubMsg(Node):
	def __init__(self):
		super().__init__('ros2_msg_pub')
		timer_period = 0.5
		timer = self.create_timer(timer_period, self.timer_callback)
		#Hull
		self.__publisher_MMSI_s = self.create_publisher(String, 'MMSI_s', 10)
		self.__publisher_WBTkLv_s = self.create_publisher(String, 'WBTkLv_s', 10)
		self.__publisher_OtherTkLv_s = self.create_publisher(String, 'OtherTkLv_s', 10)
		self.__publisher_TkLv_a = self.create_publisher(String, 'TkLv_a', 10)
		#Energy
		self.__publisher_GEN1_s = self.create_publisher(String, 'GEN1_s', 10)
		self.__publisher_GEN2_s = self.create_publisher(String, 'GEN2_s', 10)
		self.__publisher_GEN3_s = self.create_publisher(String, 'GEN3_s', 10)
		self.__publisher_GEN1_a = self.create_publisher(String, 'GEN1_a', 10)
		self.__publisher_GEN2_a = self.create_publisher(String, 'GEN2_a', 10)
		self.__publisher_GEN3_a = self.create_publisher(String, 'GEN3_a', 10)
		self.__publisher_PowSta_s = self.create_publisher(String, 'PowSta_s', 10)
		self.__publisher_PowSta_a = self.create_publisher(String, 'PowSta_a', 10)
		#Economizer
		self.__publisher_Rotor_s = self.create_publisher(String, 'Rotor_s', 10)
		self.__publisher_RotorErr_s = self.create_publisher(String, 'RotorErr_s', 10)
		self.__publisher_RotorCtrl_s = self.create_publisher(String, 'RotorCtrl_s', 10)
		self.__publisher_Rotor_a = self.create_publisher(String, 'Rotor_a', 10)
		#Power
		self.__publisher_PropMot_s = self.create_publisher(String, 'PropMot_s', 10)
		self.__publisher_PropMot_c_Speed = self.create_publisher(String, 'PropMot_c_Speed', 10)
		self.__publisher_PropMot_c_VfdRStart = self.create_publisher(String, 'PropMot_c_VfdRStart', 10)
		self.__publisher_PropMot_c_VfdRStop = self.create_publisher(String, 'PropMot_c_VfdRStop', 10)
		self.__publisher_PropMot_c_VfdRReset = self.create_publisher(String, 'PropMot_c_VfdRReset', 10)
		self.__publisher_PropMot_c_VfdRShut = self.create_publisher(String, 'PropMot_c_VfdRShut', 10)
		self.__publisher_PropMot_c_VfdEMode = self.create_publisher(String, 'PropMot_c_VfdEMode', 10)
		self.__publisher_PropMot_c_VfdEAhead = self.create_publisher(String, 'PropMot_c_VfdEAhead', 10)
		self.__publisher_PropMot_c_VfdEAstern = self.create_publisher(String, 'PropMot_c_VfdEAstern', 10)
		self.__publisher_PropMot_c_VfdEAcc = self.create_publisher(String, 'PropMot_c_VfdEAcc', 10)
		self.__publisher_PropMot_c_VfdEDec = self.create_publisher(String, 'PropMot_c_VfdEDec', 10)
		self.__publisher_PropMot_c_VfdSlowOC = self.create_publisher(String, 'PropMot_c_VfdSlowOC', 10)
		self.__publisher_PropMot_c_VfdStopOC = self.create_publisher(String, 'PropMot_c_VfdStopOC', 10)
		self.__publisher_PropMot_c_Spare = self.create_publisher(String, 'PropMot_c_Spare', 10)
		self.__publisher_PropMot_a = self.create_publisher(String, 'PropMot_a', 10)
		self.__publisher_Sft1_s = self.create_publisher(String, 'Sft1_s', 10)
		self.__publisher_Sft2_s = self.create_publisher(String, 'Sft2_s', 10)
		self.__publisher_SideThr_s = self.create_publisher(String, 'SideThr_s', 10)
		self.__publisher_SideThr_c_Speed = self.create_publisher(String, 'SideThr_c_Speed', 10)
		self.__publisher_SideThr_c_Spare = self.create_publisher(String, 'SideThr_c_Spare', 10)
		self.__publisher_SideThr_a = self.create_publisher(String, 'SideThr_a', 10)
		self.__publisher_Str1_a = self.create_publisher(String, 'Str1_a', 10)
		self.__publisher_Str2_a = self.create_publisher(String, 'Str2_a', 10)
		self.__publisher_Pilot_s = self.create_publisher(String, 'Pilot_s', 10)
		self.__publisher_Pilot_c_StrOrder = self.create_publisher(String, 'Pilot_c_StrOrder', 10)
		self.__publisher_Pilot_c_Spare = self.create_publisher(String, 'Pilot_c_Spare', 10)
		self.__publisher_Pilot_a = self.create_publisher(String, 'Pilot_a', 10)
		self.__publisher_PropRCtrl_s = self.create_publisher(String, 'PropRCtrl_s', 10)
		self.__publisher_PropRCtrl_a = self.create_publisher(String, 'PropRCtrl_a', 10)
		#Navigation
		self.__publisher_IMU_s = self.create_publisher(String, 'IMU_s', 10)
		self.__publisher_ElecCom_s = self.create_publisher(String, 'ElecCom_s', 10)
		self.__publisher_GPS_s = self.create_publisher(String, 'GPS_s', 10)
		self.__publisher_Log_s = self.create_publisher(String, 'Log_s', 10)
		#Meteorological&Hydrologic
		self.__publisher_Ane_s = self.create_publisher(String, 'Ane_s', 10)
		self.__publisher_Depth_s = self.create_publisher(String, 'Depth_s', 10)
		#Perception
		self.__publisher_AIS_s = self.create_publisher(String, 'AIS_s', 10)
		self.__publisher_XRdr_s = self.create_publisher(String, 'XRdr_s', 10)
		#AuxiliaryMachinery
		self.__publisher_Pump_s = self.create_publisher(String, 'Pump_s', 10)
		self.__publisher_Pump_c_BallastStart = self.create_publisher(String, 'Pump_c_BallastStart', 10)
		self.__publisher_Pump_c_BallastStop = self.create_publisher(String, 'Pump_c_BallastStop', 10)
		self.__publisher_Pump_c_FireStart = self.create_publisher(String, 'Pump_c_FireStart', 10)
		self.__publisher_Pump_c_FireStop = self.create_publisher(String, 'Pump_c_FireStop', 10)
		self.__publisher_Pump_c_BilgeStart = self.create_publisher(String, 'Pump_c_BilgeStart', 10)
		self.__publisher_Pump_c_BilgeStop = self.create_publisher(String, 'Pump_c_BilgeStop', 10)
		self.__publisher_Pump_c_Spare = self.create_publisher(String, 'Pump_c_Spare', 10)
		self.__publisher_Valve_s = self.create_publisher(String, 'Valve_s', 10)
		self.__publisher_Valve_c_BallastVO = self.create_publisher(String, 'Valve_c_BallastVO', 10)
		self.__publisher_Valve_c_BallastVC = self.create_publisher(String, 'Valve_c_BallastVC', 10)
		self.__publisher_Valve_c_BilgeVO = self.create_publisher(String, 'Valve_c_BilgeVO', 10)
		self.__publisher_Valve_c_BilgeVC = self.create_publisher(String, 'Valve_c_BilgeVC', 10)
		self.__publisher_Valve_c_Spare = self.create_publisher(String, 'Valve_c_Spare', 10)
		self.__publisher_OtherAuxMach_a = self.create_publisher(String, 'OtherAuxMach_a', 10)
		#Intelligent
		self.__publisher_IePept_PeptFus_s = self.create_publisher(String, 'IePept_PeptFus_s', 10)
		self.__publisher_IeCtrl_Pilot_c_Angle = self.create_publisher(String, 'IeCtrl_Pilot_c_Angle', 10)
		self.__publisher_IeCtrl_Pilot_c_Direction = self.create_publisher(String, 'IeCtrl_Pilot_c_Direction', 10)
		self.__publisher_IeCtrl_Pilot_c_Spare = self.create_publisher(String, 'IeCtrl_Pilot_c_Spare', 10)
		self.__publisher_IeCtrl_PropMot_c_Speed = self.create_publisher(String, 'IeCtrl_PropMot_c_Speed', 10)
		self.__publisher_IeCtrl_PropMot_c_Torque = self.create_publisher(String, 'IeCtrl_PropMot_c_Torque', 10)
		self.__publisher_IeCtrl_PropMot_c_SftPower = self.create_publisher(String, 'IeCtrl_PropMot_c_SftPower', 10)
		self.__publisher_IeCtrl_PropMot_c_Spare = self.create_publisher(String, 'IeCtrl_PropMot_c_Spare', 10)
		self.__publisher_IeCtrl_ST_c_Speed = self.create_publisher(String, 'IeCtrl_ST_c_Speed', 10)
		self.__publisher_IeCtrl_ST_c_Torque = self.create_publisher(String, 'IeCtrl_ST_c_Torque', 10)
		self.__publisher_IeCtrl_ST_c_SftPower = self.create_publisher(String, 'IeCtrl_ST_c_SftPower', 10)
		self.__publisher_IeCtrl_ST_c_Spare = self.create_publisher(String, 'IeCtrl_ST_c_Spare', 10)
		self.__publisher_IeCtrl_RT_s = self.create_publisher(String, 'IeCtrl_RT_s', 10)
		self.__publisher_IeDec_RP_s = self.create_publisher(String, 'IeDec_RP_s', 10)
		self.__publisher_IeDec_NaviInfo_s = self.create_publisher(String, 'IeDec_NaviInfo_s', 10)
		#Hull
		self.MMSI_s_ins = String()
		self.WBTkLv_s_ins = String()
		self.OtherTkLv_s_ins = String()
		self.TkLv_a_ins = String()
		#Energy
		self.GEN1_s_ins = String()
		self.GEN2_s_ins = String()
		self.GEN3_s_ins = String()
		self.GEN1_a_ins = String()
		self.GEN2_a_ins = String()
		self.GEN3_a_ins = String()
		self.PowSta_s_ins = String()
		self.PowSta_a_ins = String()
		#Economizer
		self.Rotor_s_ins = String()
		self.RotorErr_s_ins = String()
		self.RotorCtrl_s_ins = String()
		self.Rotor_a_ins = String()
		#Power
		self.PropMot_s_ins = String()
		self.PropMot_c_Speed_ins = String()
		self.PropMot_c_VfdRStart_ins = String()
		self.PropMot_c_VfdRStop_ins = String()
		self.PropMot_c_VfdRReset_ins = String()
		self.PropMot_c_VfdRShut_ins = String()
		self.PropMot_c_VfdEMode_ins = String()
		self.PropMot_c_VfdEAhead_ins = String()
		self.PropMot_c_VfdEAstern_ins = String()
		self.PropMot_c_VfdEAcc_ins = String()
		self.PropMot_c_VfdEDec_ins = String()
		self.PropMot_c_VfdSlowOC_ins = String()
		self.PropMot_c_VfdStopOC_ins = String()
		self.PropMot_c_Spare_ins = String()
		self.PropMot_a_ins = String()
		self.Sft1_s_ins = String()
		self.Sft2_s_ins = String()
		self.SideThr_s_ins = String()
		self.SideThr_c_Speed_ins = String()
		self.SideThr_c_Spare_ins = String()
		self.SideThr_a_ins = String()
		self.Str1_a_ins = String()
		self.Str2_a_ins = String()
		self.Pilot_s_ins = String()
		self.Pilot_c_StrOrder_ins = String()
		self.Pilot_c_Spare_ins = String()
		self.Pilot_a_ins = String()
		self.PropRCtrl_s_ins = String()
		self.PropRCtrl_a_ins = String()
		#Navigation
		self.IMU_s_ins = String()
		self.ElecCom_s_ins = String()
		self.GPS_s_ins = String()
		self.Log_s_ins = String()
		#Meteorological&Hydrologic
		self.Ane_s_ins = String()
		self.Depth_s_ins = String()
		#Perception
		self.AIS_s_ins = String()
		self.XRdr_s_ins = String()
		#AuxiliaryMachinery
		self.Pump_s_ins = String()
		self.Pump_c_BallastStart_ins = String()
		self.Pump_c_BallastStop_ins = String()
		self.Pump_c_FireStart_ins = String()
		self.Pump_c_FireStop_ins = String()
		self.Pump_c_BilgeStart_ins = String()
		self.Pump_c_BilgeStop_ins = String()
		self.Pump_c_Spare_ins = String()
		self.Valve_s_ins = String()
		self.Valve_c_BallastVO_ins = String()
		self.Valve_c_BallastVC_ins = String()
		self.Valve_c_BilgeVO_ins = String()
		self.Valve_c_BilgeVC_ins = String()
		self.Valve_c_Spare_ins = String()
		self.OtherAuxMach_a_ins = String()
		#Intelligent
		self.IePept_PeptFus_s_ins = String()
		self.IeCtrl_Pilot_c_Angle_ins = String()
		self.IeCtrl_Pilot_c_Direction_ins = String()
		self.IeCtrl_Pilot_c_Spare_ins = String()
		self.IeCtrl_PropMot_c_Speed_ins = String()
		self.IeCtrl_PropMot_c_Torque_ins = String()
		self.IeCtrl_PropMot_c_SftPower_ins = String()
		self.IeCtrl_PropMot_c_Spare_ins = String()
		self.IeCtrl_ST_c_Speed_ins = String()
		self.IeCtrl_ST_c_Torque_ins = String()
		self.IeCtrl_ST_c_SftPower_ins = String()
		self.IeCtrl_ST_c_Spare_ins = String()
		self.IeCtrl_RT_s_ins = String()
		self.IeDec_RP_s_ins = String()
		self.IeDec_NaviInfo_s_ins = String()

	def timer_callback(self):
		#Hull
		self.MMSI_s_write()
		self.WBTkLv_s_write()
		self.OtherTkLv_s_write()
		self.TkLv_a_write()
		#Energy
		self.GEN1_s_write()
		self.GEN2_s_write()
		self.GEN3_s_write()
		self.GEN1_a_write()
		self.GEN2_a_write()
		self.GEN3_a_write()
		self.PowSta_s_write()
		self.PowSta_a_write()
		#Economizer
		self.Rotor_s_write()
		self.RotorErr_s_write()
		self.RotorCtrl_s_write()
		self.Rotor_a_write()
		#Power
		self.PropMot_s_write()
		self.PropMot_c_Speed_write()
		self.PropMot_c_VfdRStart_write()
		self.PropMot_c_VfdRStop_write()
		self.PropMot_c_VfdRReset_write()
		self.PropMot_c_VfdRShut_write()
		self.PropMot_c_VfdEMode_write()
		self.PropMot_c_VfdEAhead_write()
		self.PropMot_c_VfdEAstern_write()
		self.PropMot_c_VfdEAcc_write()
		self.PropMot_c_VfdEDec_write()
		self.PropMot_c_VfdSlowOC_write()
		self.PropMot_c_VfdStopOC_write()
		self.PropMot_c_Spare_write()
		self.PropMot_a_write()
		self.Sft1_s_write()
		self.Sft2_s_write()
		self.SideThr_s_write()
		self.SideThr_c_Speed_write()
		self.SideThr_c_Spare_write()
		self.SideThr_a_write()
		self.Str1_a_write()
		self.Str2_a_write()
		self.Pilot_s_write()
		self.Pilot_c_StrOrder_write()
		self.Pilot_c_Spare_write()
		self.Pilot_a_write()
		self.PropRCtrl_s_write()
		self.PropRCtrl_a_write()
		#Navigation
		self.IMU_s_write()
		self.ElecCom_s_write()
		self.GPS_s_write()
		self.Log_s_write()
		#Meteorological&Hydrologic
		self.Ane_s_write()
		self.Depth_s_write()
		#Perception
		self.AIS_s_write()
		self.XRdr_s_write()
		#AuxiliaryMachinery
		self.Pump_s_write()
		self.Pump_c_BallastStart_write()
		self.Pump_c_BallastStop_write()
		self.Pump_c_FireStart_write()
		self.Pump_c_FireStop_write()
		self.Pump_c_BilgeStart_write()
		self.Pump_c_BilgeStop_write()
		self.Pump_c_Spare_write()
		self.Valve_s_write()
		self.Valve_c_BallastVO_write()
		self.Valve_c_BallastVC_write()
		self.Valve_c_BilgeVO_write()
		self.Valve_c_BilgeVC_write()
		self.Valve_c_Spare_write()
		self.OtherAuxMach_a_write()
		#Intelligent
		self.IePept_PeptFus_s_write()
		self.IeCtrl_Pilot_c_Angle_write()
		self.IeCtrl_Pilot_c_Direction_write()
		self.IeCtrl_Pilot_c_Spare_write()
		self.IeCtrl_PropMot_c_Speed_write()
		self.IeCtrl_PropMot_c_Torque_write()
		self.IeCtrl_PropMot_c_SftPower_write()
		self.IeCtrl_PropMot_c_Spare_write()
		self.IeCtrl_ST_c_Speed_write()
		self.IeCtrl_ST_c_Torque_write()
		self.IeCtrl_ST_c_SftPower_write()
		self.IeCtrl_ST_c_Spare_write()
		self.IeCtrl_RT_s_write()
		self.IeDec_RP_s_write()
		self.IeDec_NaviInfo_s_write()
		self.MessagePub()

	def MessagePub(self):
		#Hull
		self.__publisher_MMSI_s.publish(self.MMSI_s_ins)
		self.__publisher_WBTkLv_s.publish(self.WBTkLv_s_ins)
		self.__publisher_OtherTkLv_s.publish(self.OtherTkLv_s_ins)
		self.__publisher_TkLv_a.publish(self.TkLv_a_ins)
		#Energy
		self.__publisher_GEN1_s.publish(self.GEN1_s_ins)
		self.__publisher_GEN2_s.publish(self.GEN2_s_ins)
		self.__publisher_GEN3_s.publish(self.GEN3_s_ins)
		self.__publisher_GEN1_a.publish(self.GEN1_a_ins)
		self.__publisher_GEN2_a.publish(self.GEN2_a_ins)
		self.__publisher_GEN3_a.publish(self.GEN3_a_ins)
		self.__publisher_PowSta_s.publish(self.PowSta_s_ins)
		self.__publisher_PowSta_a.publish(self.PowSta_a_ins)
		#Economizer
		self.__publisher_Rotor_s.publish(self.Rotor_s_ins)
		self.__publisher_RotorErr_s.publish(self.RotorErr_s_ins)
		self.__publisher_RotorCtrl_s.publish(self.RotorCtrl_s_ins)
		self.__publisher_Rotor_a.publish(self.Rotor_a_ins)
		#Power
		self.__publisher_PropMot_s.publish(self.PropMot_s_ins)
		self.__publisher_PropMot_c_Speed.publish(self.PropMot_c_Speed_ins)
		self.__publisher_PropMot_c_VfdRStart.publish(self.PropMot_c_VfdRStart_ins)
		self.__publisher_PropMot_c_VfdRStop.publish(self.PropMot_c_VfdRStop_ins)
		self.__publisher_PropMot_c_VfdRReset.publish(self.PropMot_c_VfdRReset_ins)
		self.__publisher_PropMot_c_VfdRShut.publish(self.PropMot_c_VfdRShut_ins)
		self.__publisher_PropMot_c_VfdEMode.publish(self.PropMot_c_VfdEMode_ins)
		self.__publisher_PropMot_c_VfdEAhead.publish(self.PropMot_c_VfdEAhead_ins)
		self.__publisher_PropMot_c_VfdEAstern.publish(self.PropMot_c_VfdEAstern_ins)
		self.__publisher_PropMot_c_VfdEAcc.publish(self.PropMot_c_VfdEAcc_ins)
		self.__publisher_PropMot_c_VfdEDec.publish(self.PropMot_c_VfdEDec_ins)
		self.__publisher_PropMot_c_VfdSlowOC.publish(self.PropMot_c_VfdSlowOC_ins)
		self.__publisher_PropMot_c_VfdStopOC.publish(self.PropMot_c_VfdStopOC_ins)
		self.__publisher_PropMot_c_Spare.publish(self.PropMot_c_Spare_ins)
		self.__publisher_PropMot_a.publish(self.PropMot_a_ins)
		self.__publisher_Sft1_s.publish(self.Sft1_s_ins)
		self.__publisher_Sft2_s.publish(self.Sft2_s_ins)
		self.__publisher_SideThr_s.publish(self.SideThr_s_ins)
		self.__publisher_SideThr_c_Speed.publish(self.SideThr_c_Speed_ins)
		self.__publisher_SideThr_c_Spare.publish(self.SideThr_c_Spare_ins)
		self.__publisher_SideThr_a.publish(self.SideThr_a_ins)
		self.__publisher_Str1_a.publish(self.Str1_a_ins)
		self.__publisher_Str2_a.publish(self.Str2_a_ins)
		self.__publisher_Pilot_s.publish(self.Pilot_s_ins)
		self.__publisher_Pilot_c_StrOrder.publish(self.Pilot_c_StrOrder_ins)
		self.__publisher_Pilot_c_Spare.publish(self.Pilot_c_Spare_ins)
		self.__publisher_Pilot_a.publish(self.Pilot_a_ins)
		self.__publisher_PropRCtrl_s.publish(self.PropRCtrl_s_ins)
		self.__publisher_PropRCtrl_a.publish(self.PropRCtrl_a_ins)
		#Navigation
		self.__publisher_IMU_s.publish(self.IMU_s_ins)
		self.__publisher_ElecCom_s.publish(self.ElecCom_s_ins)
		self.__publisher_GPS_s.publish(self.GPS_s_ins)
		self.__publisher_Log_s.publish(self.Log_s_ins)
		#Meteorological&Hydrologic
		self.__publisher_Ane_s.publish(self.Ane_s_ins)
		self.__publisher_Depth_s.publish(self.Depth_s_ins)
		#Perception
		self.__publisher_AIS_s.publish(self.AIS_s_ins)
		self.__publisher_XRdr_s.publish(self.XRdr_s_ins)
		#AuxiliaryMachinery
		self.__publisher_Pump_s.publish(self.Pump_s_ins)
		self.__publisher_Pump_c_BallastStart.publish(self.Pump_c_BallastStart_ins)
		self.__publisher_Pump_c_BallastStop.publish(self.Pump_c_BallastStop_ins)
		self.__publisher_Pump_c_FireStart.publish(self.Pump_c_FireStart_ins)
		self.__publisher_Pump_c_FireStop.publish(self.Pump_c_FireStop_ins)
		self.__publisher_Pump_c_BilgeStart.publish(self.Pump_c_BilgeStart_ins)
		self.__publisher_Pump_c_BilgeStop.publish(self.Pump_c_BilgeStop_ins)
		self.__publisher_Pump_c_Spare.publish(self.Pump_c_Spare_ins)
		self.__publisher_Valve_s.publish(self.Valve_s_ins)
		self.__publisher_Valve_c_BallastVO.publish(self.Valve_c_BallastVO_ins)
		self.__publisher_Valve_c_BallastVC.publish(self.Valve_c_BallastVC_ins)
		self.__publisher_Valve_c_BilgeVO.publish(self.Valve_c_BilgeVO_ins)
		self.__publisher_Valve_c_BilgeVC.publish(self.Valve_c_BilgeVC_ins)
		self.__publisher_Valve_c_Spare.publish(self.Valve_c_Spare_ins)
		self.__publisher_OtherAuxMach_a.publish(self.OtherAuxMach_a_ins)
		#Intelligent
		self.__publisher_IePept_PeptFus_s.publish(self.IePept_PeptFus_s_ins)
		self.__publisher_IeCtrl_Pilot_c_Angle.publish(self.IeCtrl_Pilot_c_Angle_ins)
		self.__publisher_IeCtrl_Pilot_c_Direction.publish(self.IeCtrl_Pilot_c_Direction_ins)
		self.__publisher_IeCtrl_Pilot_c_Spare.publish(self.IeCtrl_Pilot_c_Spare_ins)
		self.__publisher_IeCtrl_PropMot_c_Speed.publish(self.IeCtrl_PropMot_c_Speed_ins)
		self.__publisher_IeCtrl_PropMot_c_Torque.publish(self.IeCtrl_PropMot_c_Torque_ins)
		self.__publisher_IeCtrl_PropMot_c_SftPower.publish(self.IeCtrl_PropMot_c_SftPower_ins)
		self.__publisher_IeCtrl_PropMot_c_Spare.publish(self.IeCtrl_PropMot_c_Spare_ins)
		self.__publisher_IeCtrl_ST_c_Speed.publish(self.IeCtrl_ST_c_Speed_ins)
		self.__publisher_IeCtrl_ST_c_Torque.publish(self.IeCtrl_ST_c_Torque_ins)
		self.__publisher_IeCtrl_ST_c_SftPower.publish(self.IeCtrl_ST_c_SftPower_ins)
		self.__publisher_IeCtrl_ST_c_Spare.publish(self.IeCtrl_ST_c_Spare_ins)
		self.__publisher_IeCtrl_RT_s.publish(self.IeCtrl_RT_s_ins)
		self.__publisher_IeDec_RP_s.publish(self.IeDec_RP_s_ins)
		self.__publisher_IeDec_NaviInfo_s.publish(self.IeDec_NaviInfo_s_ins)

		#Hull
	def MMSI_s_write(self):
		self.MMSI_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'MMSI':str(random.randint(100000000, 999999999))
		})
	def WBTkLv_s_write(self):
		self.WBTkLv_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'bottom1_l': round(random.uniform(0,10),2),
		'bottom1_r': round(random.uniform(0,10),2),
		'bottom2_l': round(random.uniform(0,10),2),
		'bottom2_r': round(random.uniform(0,10),2),
		'bottom3_l': round(random.uniform(0,10),2),
		'bottom3_r': round(random.uniform(0,10),2),
		'bottom4_l': round(random.uniform(0,10),2),
		'bottom4_r': round(random.uniform(0,10),2),
		'bottom5_l': round(random.uniform(0,10),2),
		'bottom5_r': round(random.uniform(0,10),2),
		'bottom6_l': round(random.uniform(0,10),2),
		'bottom6_r': round(random.uniform(0,10),2),
		'bottom7_l': round(random.uniform(0,10),2),
		'bottom7_r': round(random.uniform(0,10),2),
		'fore_peak': round(random.uniform(0,10),2),
		'aft_peak_l': round(random.uniform(0,10),2),
		'aft_peak_r': round(random.uniform(0,10),2),
		'tank1_l': round(random.uniform(0,10),2),
		'tank1_r': round(random.uniform(0,10),2),
		'tank2_l': round(random.uniform(0,10),2),
		'tank2_r': round(random.uniform(0,10),2),
		'tank4_l': round(random.uniform(0,10),2),
		'tank4_r': round(random.uniform(0,10),2),
		'tank5_l': round(random.uniform(0,10),2),
		'tank5_r': round(random.uniform(0,10),2),
		'tank6_l': round(random.uniform(0,10),2),
		'tank6_r': round(random.uniform(0,10),2),
		'tank7_l': round(random.uniform(0,10),2),
		'tank7_r': round(random.uniform(0,10),2),
		'spare': round(random.uniform(0,10),2),
		})
	def OtherTkLv_s_write(self):
		self.OtherTkLv_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'fuel_oil_l': round(random.uniform(0,10),2),
		'fuel_oil_r': round(random.uniform(0,10),2),
		'water_l': round(random.uniform(0,10),2),
		'water_r': round(random.uniform(0,10),2),
		'fuel_oil_daily_l': round(random.uniform(0,10),2),
		'fuel_oil_daily_r': round(random.uniform(0,10),2),
		'spare': round(random.uniform(0,10),2),
		})
	def TkLv_a_write(self):
		self.TkLv_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'SteerEngineRoomHL': bool(random.randint(0,1)),
		'PropulEquipRoom_Rear_HL': bool(random.randint(0,1)),
		'PropulEquipRoom_Front_HL': bool(random.randint(0,1)),
		'EngineRoom_LeftRear_HL': bool(random.randint(0,1)),
		'EngineRoom_FrontLeft_HL': bool(random.randint(0,1)),
		'EngineRoom_RearRight_HL': bool(random.randint(0,1)),
		'AirCondRoom_MiddleRear_HL': bool(random.randint(0,1)),
		'StorageRoom_MiddleRear_HL': bool(random.randint(0,1)),
		'StorageRoom_MiddleFront_HL': bool(random.randint(0,1)),
		'SideThrustRoom_MiddleRear_HL': bool(random.randint(0,1)),
		'SideThrustRoom_MiddleFront_HL': bool(random.randint(0,1)),
		'LogRoomHL': bool(random.randint(0,1)),
		'DepthometerRoomHL': bool(random.randint(0,1)),
		'StorageRoom_LeftRear_HL': bool(random.randint(0,1)),
		'StorageRoom_RearRight_HL': bool(random.randint(0,1)),
		'ChainLocker_Left_HL': bool(random.randint(0,1)),
		'ChainLocker_Right_HL': bool(random.randint(0,1)),
		'FOTk1_Left_HL': bool(random.randint(0,1)),
		'FOTk1_Right_HL': bool(random.randint(0,1)),
		'FODTk1_Left_HL': bool(random.randint(0,1)),
		'FODTk1_Left_LL': bool(random.randint(0,1)),
		'SewageTkHL': bool(random.randint(0,1)),
		'FODTk_Right_HL': bool(random.randint(0,1)),
		'FODTk_Right_LL': bool(random.randint(0,1)),
		'SternTubeFrontSealTk': bool(random.randint(0,1)),
		'SternTubeAfterSealTk': bool(random.randint(0,1)),
		'SlopTkHL': bool(random.randint(0,1)),
		'FOOverflowHHL': bool(random.randint(0,1)),
		'FOOverflowHL': bool(random.randint(0,1)),
		'BilgeTkHL': bool(random.randint(0,1)),
		'IntegratedAlarm': bool(random.randint(0,1)),
		'FreshWaterPressTkIntegrated': bool(random.randint(0,1)),
		'HotWaterPressTkIntegrated': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})

		#Energy
	def GEN1_s_write(self):
		self.GEN1_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'power': round(random.uniform(0,99),2),
		'intake_temp': round(random.uniform(0,99),2),
		'intake_press': round(random.uniform(0,99),2),
		'cool_water_temp': round(random.uniform(0,99),2),
		'camshaft_speed': round(random.uniform(0,99),2),
		'crank_shaft_speed': round(random.uniform(0,99),2),
		'u_winding_temp': round(random.uniform(0,99),2),
		'v_winding_temp': round(random.uniform(0,99),2),
		'w_winding_temp': round(random.uniform(0,99),2),
		'non_driving_bearing_temp': round(random.uniform(0,99),2),
		'cool_water_press': round(random.uniform(0,99),2),
		'engine_oil_temp': round(random.uniform(0,99),2),
		'engine_oil_press': round(random.uniform(0,99),2),
		'charging_voltage': round(random.uniform(0,99),2),
		'battery_voltage': round(random.uniform(0,99),2),
		'ship_supply_voltage': round(random.uniform(0,99),2),
		'security_speed': round(random.uniform(0,99),2),
		'security_press': round(random.uniform(0,99),2),
		'sea_water_press': round(random.uniform(0,99),2),
		'oil_before_filter_press': round(random.uniform(0,99),2),
		'fuel_press': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def GEN2_s_write(self):
		self.GEN2_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'power': round(random.uniform(0,99),2),
		'intake_temp': round(random.uniform(0,99),2),
		'intake_press': round(random.uniform(0,99),2),
		'cool_water_temp': round(random.uniform(0,99),2),
		'camshaft_speed': round(random.uniform(0,99),2),
		'crank_shaft_speed': round(random.uniform(0,99),2),
		'u_winding_temp': round(random.uniform(0,99),2),
		'v_winding_temp': round(random.uniform(0,99),2),
		'w_winding_temp': round(random.uniform(0,99),2),
		'non_driving_bearing_temp': round(random.uniform(0,99),2),
		'cool_water_press': round(random.uniform(0,99),2),
		'engine_oil_temp': round(random.uniform(0,99),2),
		'engine_oil_press': round(random.uniform(0,99),2),
		'charging_voltage': round(random.uniform(0,99),2),
		'battery_voltage': round(random.uniform(0,99),2),
		'ship_supply_voltage': round(random.uniform(0,99),2),
		'security_speed': round(random.uniform(0,99),2),
		'security_press': round(random.uniform(0,99),2),
		'sea_water_press': round(random.uniform(0,99),2),
		'oil_before_filter_press': round(random.uniform(0,99),2),
		'fuel_press': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def GEN3_s_write(self):
		self.GEN3_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'power': round(random.uniform(0,99),2),
		'intake_temp': round(random.uniform(0,99),2),
		'intake_press': round(random.uniform(0,99),2),
		'cool_water_temp': round(random.uniform(0,99),2),
		'camshaft_speed': round(random.uniform(0,99),2),
		'crank_shaft_speed': round(random.uniform(0,99),2),
		'u_winding_temp': round(random.uniform(0,99),2),
		'v_winding_temp': round(random.uniform(0,99),2),
		'w_winding_temp': round(random.uniform(0,99),2),
		'non_driving_bearing_temp': round(random.uniform(0,99),2),
		'cool_water_press': round(random.uniform(0,99),2),
		'engine_oil_temp': round(random.uniform(0,99),2),
		'engine_oil_press': round(random.uniform(0,99),2),
		'charging_voltage': round(random.uniform(0,99),2),
		'battery_voltage': round(random.uniform(0,99),2),
		'ship_supply_voltage': round(random.uniform(0,99),2),
		'security_speed': round(random.uniform(0,99),2),
		'security_press': round(random.uniform(0,99),2),
		'sea_water_press': round(random.uniform(0,99),2),
		'oil_before_filter_press': round(random.uniform(0,99),2),
		'fuel_press': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def GEN1_a_write(self):
		self.GEN1_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'ChargeFailure': bool(random.randint(0,1)),
		'FuelLeakage': bool(random.randint(0,1)),
		'StartFailure': bool(random.randint(0,1)),
		'LowWaterPressure': bool(random.randint(0,1)),
		'LowWaterLevel': bool(random.randint(0,1)),
		'PowerSupplyUnderVoltage': bool(random.randint(0,1)),
		'HighOilTemperature': bool(random.randint(0,1)),
		'HighWaterTemperature': bool(random.randint(0,1)),
		'LowOilPressure': bool(random.randint(0,1)),
		'SpeedAlarm': bool(random.randint(0,1)),
		'OverSpeedShutdown': bool(random.randint(0,1)),
		'LowOilPressureShutdown': bool(random.randint(0,1)),
		'LowFuelInletPressure': bool(random.randint(0,1)),
		'HighFilterDifferentialPressure': bool(random.randint(0,1)),
		'LowSeaWaterPressure': bool(random.randint(0,1)),
		'LowBatteryVoltage': bool(random.randint(0,1)),
		'LowShipPowerSupplyVoltage': bool(random.randint(0,1)),
		'LowCoolingLevel': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})
	def GEN2_a_write(self):
		self.GEN2_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'ChargeFailure': bool(random.randint(0,1)),
		'FuelLeakage': bool(random.randint(0,1)),
		'StartFailure': bool(random.randint(0,1)),
		'LowWaterPressure': bool(random.randint(0,1)),
		'LowWaterLevel': bool(random.randint(0,1)),
		'PowerSupplyUnderVoltage': bool(random.randint(0,1)),
		'HighOilTemperature': bool(random.randint(0,1)),
		'HighWaterTemperature': bool(random.randint(0,1)),
		'LowOilPressure': bool(random.randint(0,1)),
		'SpeedAlarm': bool(random.randint(0,1)),
		'OverSpeedShutdown': bool(random.randint(0,1)),
		'LowOilPressureShutdown': bool(random.randint(0,1)),
		'LowFuelInletPressure': bool(random.randint(0,1)),
		'HighFilterDifferentialPressure': bool(random.randint(0,1)),
		'LowSeaWaterPressure': bool(random.randint(0,1)),
		'LowBatteryVoltage': bool(random.randint(0,1)),
		'LowShipPowerSupplyVoltage': bool(random.randint(0,1)),
		'LowCoolingLevel': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})
	def GEN3_a_write(self):
		self.GEN3_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'ChargeFailure': bool(random.randint(0,1)),
		'FuelLeakage': bool(random.randint(0,1)),
		'StartFailure': bool(random.randint(0,1)),
		'LowWaterPressure': bool(random.randint(0,1)),
		'LowWaterLevel': bool(random.randint(0,1)),
		'PowerSupplyUnderVoltage': bool(random.randint(0,1)),
		'HighOilTemperature': bool(random.randint(0,1)),
		'HighWaterTemperature': bool(random.randint(0,1)),
		'LowOilPressure': bool(random.randint(0,1)),
		'SpeedAlarm': bool(random.randint(0,1)),
		'OverSpeedShutdown': bool(random.randint(0,1)),
		'LowOilPressureShutdown': bool(random.randint(0,1)),
		'LowFuelInletPressure': bool(random.randint(0,1)),
		'HighFilterDifferentialPressure': bool(random.randint(0,1)),
		'LowSeaWaterPressure': bool(random.randint(0,1)),
		'LowBatteryVoltage': bool(random.randint(0,1)),
		'LowShipPowerSupplyVoltage': bool(random.randint(0,1)),
		'LowCoolingLevel': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})
	def PowSta_s_write(self):
		self.PowSta_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'gen_current_1': round(random.uniform(0,99),2),
		'gen_power_1': round(random.uniform(0,99),2),
		'gen_current_2': round(random.uniform(0,99),2),
		'gen_power_2': round(random.uniform(0,99),2),
		'gen_current_3': round(random.uniform(0,99),2),
		'gen_power_3': round(random.uniform(0,99),2),
		'residue_power': round(random.uniform(0,99),2),
		'aircond_swp1_run': bool(random.randint(0,1)),
		'dirty_oil_p_run': bool(random.randint(0,1)),
		'diesel_p1_run': bool(random.randint(0,1)),
		'bilge_ballast_p_run': bool(random.randint(0,1)),
		'er_fan1_run': bool(random.randint(0,1)),
		'sewage_p_run': bool(random.randint(0,1)),
		'aircond_swp2_run': bool(random.randint(0,1)),
		'daily_bilge_p_run': bool(random.randint(0,1)),
		'diesel_p2_run': bool(random.randint(0,1)),
		'fire_ballast_p_run': bool(random.randint(0,1)),
		'er_fan2_run': bool(random.randint(0,1)),
		'spare_p_run': bool(random.randint(0,1)),
		'spare': round(random.uniform(0,99),2),
		})
	def PowSta_a_write(self):
		self.PowSta_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'No1GeneratorOverloadWarning': bool(random.randint(0,1)),
		'No1GeneratorStartFailure': bool(random.randint(0,1)),
		'No1GeneratorAbnormalTrip': bool(random.randint(0,1)),
		'No1GeneratorClosingFailure': bool(random.randint(0,1)),
		'No1GeneratorInversePower': bool(random.randint(0,1)),
		'No1GeneratorSwitchingFailure': bool(random.randint(0,1)),
		'No1GeneratorPMSIntegratedAlarm': bool(random.randint(0,1)),
		'No1GeneratorEmergencyStop': bool(random.randint(0,1)),
		'No1GeneratorSetIntegratedAlarm': bool(random.randint(0,1)),
		'No2GeneratorOverloadWarning': bool(random.randint(0,1)),
		'No2GeneratorStartFailure': bool(random.randint(0,1)),
		'No2GeneratorAbnormalTrip': bool(random.randint(0,1)),
		'No2GeneratorClosingFailure': bool(random.randint(0,1)),
		'No2GeneratorInversePower': bool(random.randint(0,1)),
		'No2GeneratorSwitchingFailure': bool(random.randint(0,1)),
		'No2GeneratorPMSIntegratedAlarm': bool(random.randint(0,1)),
		'No2GeneratorEmergencyStop': bool(random.randint(0,1)),
		'No2GeneratorSetIntegratedAlarm': bool(random.randint(0,1)),
		'No3GeneratorOverloadWarning': bool(random.randint(0,1)),
		'No3GeneratorStartFailure': bool(random.randint(0,1)),
		'No3GeneratorAbnormalTrip': bool(random.randint(0,1)),
		'No3GeneratorClosingFailure': bool(random.randint(0,1)),
		'No3GeneratorInversePower': bool(random.randint(0,1)),
		'No3GeneratorSwitchingFailure': bool(random.randint(0,1)),
		'No3GeneratorPMSIntegratedAlarm': bool(random.randint(0,1)),
		'No3GeneratorEmergencyStop': bool(random.randint(0,1)),
		'No3GeneratorSetIntegratedAlarm': bool(random.randint(0,1)),
		'HighBusbarVoltageAlarm': bool(random.randint(0,1)),
		'HighBusbarVoltageTrip': bool(random.randint(0,1)),
		'LowBusbarVoltageAlarm': bool(random.randint(0,1)),
		'LowBusbarVoltageTrip': bool(random.randint(0,1)),
		'HighBusbarFrequencyAlarm': bool(random.randint(0,1)),
		'HighBusbarFrequencyTrip': bool(random.randint(0,1)),
		'LowBusbarFrequencyAlarm': bool(random.randint(0,1)),
		'LowBusbarFrequencyTrip': bool(random.randint(0,1)),
		'AutomaticSynchronizationFailure': bool(random.randint(0,1)),
		'LightLoadInhibition': bool(random.randint(0,1)),
		'ExternalDC24VPowerFault': bool(random.randint(0,1)),
		'SwitchboardDC24VPowerFault': bool(random.randint(0,1)),
		'AC400VLowInsulation': bool(random.randint(0,1)),
		'AC220VLowInsulation': bool(random.randint(0,1)),
		'EmergencyStop_PriorityTripPowerFault': bool(random.randint(0,1)),
		'PriorityTrip': bool(random.randint(0,1)),
		'SideThrusterInquiryFailure': bool(random.randint(0,1)),
		'SideThrusterRunFailure': bool(random.randint(0,1)),
		'DisconnectedAlarm': bool(random.randint(0,1)),
		'CommunicationFailure': bool(random.randint(0,1)),
		'ResultantFault': bool(random.randint(0,1)),
		'AC220V1InputFault': bool(random.randint(0,1)),
		'UPS1OutputFault': bool(random.randint(0,1)),
		'AC220V2InputFault': bool(random.randint(0,1)),
		'UPS2OutputFault': bool(random.randint(0,1)),
		'DC24VInputFault': bool(random.randint(0,1)),
		'MSBDC24VPowerFault': bool(random.randint(0,1)),
		'MSBPMSFault': bool(random.randint(0,1)),
		'MSBIntegratedAlarm': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})

		#Economizer
	def Rotor_s_write(self):
		self.Rotor_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'record_time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'wap_runt': str(random.randint(1000000,9999999)),
		'wap_c_runt': str(random.randint(1000000,9999999)),
		'wap_code': str(random.randint(1000000,9999999)),
		'wap_power': round(random.uniform(0,99),2),
		'wap_thrust': round(random.uniform(0,99),2),
		'wap_direct': str(random.randint(1000000,9999999)),
		'wap_speed': round(random.uniform(0,99),2),
		'wap_e_cost': round(random.uniform(0,99),2),
		'wap_e_direct': str(random.randint(1000000,9999999)),
		'wap_e_speed': round(random.uniform(0,99),2),
		'wap_e_power': round(random.uniform(0,99),2),
		'i_t_s_strain': round(random.uniform(0,99),2),
		'wheel_f': round(random.uniform(0,99),2),
		'i_t_strain': round(random.uniform(0,99),2),
		'f_strain': round(random.uniform(0,99),2),
		'r_strain': round(random.uniform(0,99),2),
		'bi_strain': round(random.uniform(0,99),2),
		'be_strain': round(random.uniform(0,99),2),
		'b_crack': bool(random.randint(0,1)),
		'r_crack': bool(random.randint(0,1)),
		'r_deformation': round(random.uniform(0,99),2),
		's_deflection': round(random.uniform(0,99),2),
		'f_vibration': round(random.uniform(0,99),2),
		'b_vibration': round(random.uniform(0,99),2),
		'e_vibration': round(random.uniform(0,99),2),
		'inner_noise': round(random.uniform(0,99),2),
		'e_tem': round(random.uniform(0,99),2),
		'b_tem': round(random.uniform(0,99),2),
		'wheel_tem': round(random.uniform(0,99),2),
		'inner_tem': round(random.uniform(0,99),2),
		'spare': str(random.randint(1000000,9999999)),
		})
	def RotorErr_s_write(self):
		self.RotorErr_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'record_time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'error_no': str(random.randint(1000000,9999999)),
		'error_code': str(random.randint(1000000,9999999)),
		'error_u_code': str(random.randint(1000000,9999999)),
		'error_unit': str(random.randint(1000000,9999999)),
		'error_info': str(random.randint(1000000,9999999)),
		'error_r_time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'error_e_time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': str(random.randint(1000000,9999999)),
		})
	def RotorCtrl_s_write(self):
		self.RotorCtrl_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'record_time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'cmd_name': str(random.randint(1000000,9999999)),
		'cmd_s': bool(random.randint(0,1)),
		'cmd_timeout': bool(random.randint(0,1)),
		'cmd_time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'task_id': str(random.randint(1000000,9999999)),
		'spare': str(random.randint(1000000,9999999)),
		})
	def Rotor_a_write(self):
		self.Rotor_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'alarm': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})

		#Power
	def PropMot_s_write(self):
		self.PropMot_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_local_mode': bool(random.randint(0,1)),
		'vfd_remote_mode': bool(random.randint(0,1)),
		'vfd_intelligent_mode': bool(random.randint(0,1)),
		'l_winding_running': bool(random.randint(0,1)),
		'r_winding_running': bool(random.randint(0,1)),
		'l_winding_switch': bool(random.randint(0,1)),
		'r_winding_switch': bool(random.randint(0,1)),
		'vfd_backup': bool(random.randint(0,1)),
		'vfd_shutdown': bool(random.randint(0,1)),
		'vfd_slowdown_octrl': bool(random.randint(0,1)),
		'vfd_shutdown_octrl': bool(random.randint(0,1)),
		'vfd_power_limit': bool(random.randint(0,1)),
		'vfd_emergency': bool(random.randint(0,1)),
		'speed': round(random.uniform(0,99),2),
		'l_winding_voltage': round(random.uniform(0,99),2),
		'r_winding_voltage': round(random.uniform(0,99),2),
		'l_winding_current': round(random.uniform(0,99),2),
		'r_winding_current': round(random.uniform(0,99),2),
		'l_U_temp': round(random.uniform(0,99),2),
		'l_V_temp': round(random.uniform(0,99),2),
		'l_W_temp': round(random.uniform(0,99),2),
		'r_U_temp': round(random.uniform(0,99),2),
		'r_V_temp': round(random.uniform(0,99),2),
		'r_W_temp': round(random.uniform(0,99),2),
		'driving_bearing_temp': round(random.uniform(0,99),2),
		'non_driving_bearing_temp': round(random.uniform(0,99),2),
		'l_winding_power': round(random.uniform(0,99),2),
		'r_winding_power': round(random.uniform(0,99),2),
		'l_vfd_voltage': round(random.uniform(0,99),2),
		'r_vfd_voltage': round(random.uniform(0,99),2),
		'l_vfd_current': round(random.uniform(0,99),2),
		'r_vfd_current': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def PropMot_c_Speed_write(self):
		self.PropMot_c_Speed_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'speed': round(random.uniform(0,99),2),
		})
	def PropMot_c_VfdRStart_write(self):
		self.PropMot_c_VfdRStart_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_r_start': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdRStop_write(self):
		self.PropMot_c_VfdRStop_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_r_stop': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdRReset_write(self):
		self.PropMot_c_VfdRReset_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_r_reset': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdRShut_write(self):
		self.PropMot_c_VfdRShut_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_r_shutdown': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdEMode_write(self):
		self.PropMot_c_VfdEMode_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_e_mode': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdEAhead_write(self):
		self.PropMot_c_VfdEAhead_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_e_ahead': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdEAstern_write(self):
		self.PropMot_c_VfdEAstern_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_e_astern': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdEAcc_write(self):
		self.PropMot_c_VfdEAcc_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_e_acc': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdEDec_write(self):
		self.PropMot_c_VfdEDec_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_e_dec': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdSlowOC_write(self):
		self.PropMot_c_VfdSlowOC_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_slowdown_octrl': bool(random.randint(0,1)),
		})
	def PropMot_c_VfdStopOC_write(self):
		self.PropMot_c_VfdStopOC_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'vfd_stop_octrl': bool(random.randint(0,1)),
		})
	def PropMot_c_Spare_write(self):
		self.PropMot_c_Spare_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': round(random.uniform(0,99),2),
		})
	def PropMot_a_write(self):
		self.PropMot_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'HighWindingPhaseVoltage': bool(random.randint(0,1)),
		'HighLeftWindingTemprature': bool(random.randint(0,1)),
		'HighRightWindingTemprature': bool(random.randint(0,1)),
		'HighDrivingBearingTemprature': bool(random.randint(0,1)),
		'HighNon_drivingBearingTemprature': bool(random.randint(0,1)),
		'LeftWindingOverload': bool(random.randint(0,1)),
		'RightWindingOverload': bool(random.randint(0,1)),
		'CoolingFanFeedbackAbnormal': bool(random.randint(0,1)),
		'VFCFanFailure': bool(random.randint(0,1)),
		'VFCLowInsulation': bool(random.randint(0,1)),
		'FaultDecelerationAlarm': bool(random.randint(0,1)),
		'FaultShutdownAlarm': bool(random.randint(0,1)),
		'WindingOvertemperature': bool(random.randint(0,1)),
		'DrivingBearingOvertemprature': bool(random.randint(0,1)),
		'Non_drivingBearingOvertemprature': bool(random.randint(0,1)),
		'DelayedOvercurrent': bool(random.randint(0,1)),
		'InstantaneousOvercurrent': bool(random.randint(0,1)),
		'FilterFuseFusing': bool(random.randint(0,1)),
		'PowerModuleAlarm': bool(random.randint(0,1)),
		'MotorOverspeed': bool(random.randint(0,1)),
		'StartupFailure': bool(random.randint(0,1)),
		'EmergencyStop': bool(random.randint(0,1)),
		'IntegratedAlarm': bool(random.randint(0,1)),
		'LeftWindingDelayOvercurrentWarn': bool(random.randint(0,1)),
		'RightWindingDelayOvercurrentWarn': bool(random.randint(0,1)),
		'IntegratedWarning': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})
	def Sft1_s_write(self):
		self.Sft1_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'speed': round(random.uniform(0,1000),2),
		'torque': round(random.uniform(0,1000),2),
		'power': round(random.uniform(0,5000),2),
		'thrust': round(random.uniform(0,1000),2),
		'stn_sft_rear_bearing_temp': round(random.uniform(0,99),2),
		'thrust_bearing_temp': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def Sft2_s_write(self):
		self.Sft2_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'speed': round(random.uniform(0,1000),2),
		'torque': round(random.uniform(0,1000),2),
		'power': round(random.uniform(0,5000),2),
		'thrust': round(random.uniform(0,1000),2),
		'stn_sft_rear_bearing_temp': round(random.uniform(0,99),2),
		'thrust_bearing_temp': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def SideThr_s_write(self):
		self.SideThr_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'speed': round(random.uniform(0,500),2),
		'spare': round(random.uniform(0,99),2),
		})
	def SideThr_c_Speed_write(self):
		self.SideThr_c_Speed_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'speed': round(random.uniform(0,500),2),
		})
	def SideThr_c_Spare_write(self):
		self.SideThr_c_Spare_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': round(random.uniform(0,99),2),
		})
	def SideThr_a_write(self):
		self.SideThr_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'ControlPowerFault': bool(random.randint(0,1)),
		'SparePowerFault': bool(random.randint(0,1)),
		'LowGravityOilLevel': bool(random.randint(0,1)),
		'HighMotorTemperature': bool(random.randint(0,1)),
		'IntegratedFault': bool(random.randint(0,1)),
		'Follow_upFault': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})
	def Str1_a_write(self):
		self.Str1_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'PowerLoss': bool(random.randint(0,1)),
		'ControllerPowerLoss': bool(random.randint(0,1)),
		'Power': bool(random.randint(0,1)),
		'Running': bool(random.randint(0,1)),
		'LowTankLevel': bool(random.randint(0,1)),
		'HighTankOilTemperature': bool(random.randint(0,1)),
		'PowerPhaseDisconnection': bool(random.randint(0,1)),
		'MotorOverload': bool(random.randint(0,1)),
		'HydraulicBlockage': bool(random.randint(0,1)),
		'LossOilPressure': bool(random.randint(0,1)),
		'FilterClogging': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})
	def Str2_a_write(self):
		self.Str2_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'PowerLoss': bool(random.randint(0,1)),
		'ControllerPowerLoss': bool(random.randint(0,1)),
		'Power': bool(random.randint(0,1)),
		'Running': bool(random.randint(0,1)),
		'LowTankLevel': bool(random.randint(0,1)),
		'HighTankOilTemperature': bool(random.randint(0,1)),
		'PowerPhaseDisconnection': bool(random.randint(0,1)),
		'MotorOverload': bool(random.randint(0,1)),
		'HydraulicBlockage': bool(random.randint(0,1)),
		'LossOilPressure': bool(random.randint(0,1)),
		'FilterClogging': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})
	def Pilot_s_write(self):
		self.Pilot_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'set_head': round(random.uniform(0,360),2),
		'actual_head': round(random.uniform(0,360),2),
		'actual_angle': round(random.uniform(0,360),2),
		'spare': round(random.uniform(0,99),2),
		})
	def Pilot_c_StrOrder_write(self):
		self.Pilot_c_StrOrder_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'angle': round(random.uniform(0,70),2),
		'direction': 'L/R',
		})
	def Pilot_c_Spare_write(self):
		self.Pilot_c_Spare_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': round(random.uniform(0,99),2),
		})
	def Pilot_a_write(self):
		self.Pilot_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'YawAlarm': bool(random.randint(0,1)),
		'MCUFault': bool(random.randint(0,1)),
		'ControllerTimeout': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})
	def PropRCtrl_s_write(self):
		self.PropRCtrl_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'sail_mode': bool(random.randint(0,1)),
		'control_mode': bool(random.randint(0,1)),
		'operate_mode': bool(random.randint(0,1)),
		'stop_octrl': bool(random.randint(0,1)),
		'slowdown_octrl': bool(random.randint(0,1)),
		'spare': round(random.uniform(0,99),2),
		})
	def PropRCtrl_a_write(self):
		self.PropRCtrl_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'FrontHandleDisconnection': bool(random.randint(0,1)),
		'Follow_upControlFailure': bool(random.randint(0,1)),
		'FrontHandleEmergencyStop': bool(random.randint(0,1)),
		'SideControlEmergencyStop': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})

		#Navigation
	def IMU_s_write(self):
		self.IMU_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'x_acceleration': round(random.uniform(0,20),2),
		'y_acceleration': round(random.uniform(0,20),2),
		'z_acceleration': round(random.uniform(0,20),2),
		'x_angular_velocity': round(random.uniform(0,10),2),
		'y_angular_velocity': round(random.uniform(0,10),2),
		'z_angular_velocity': round(random.uniform(0,10),2),
		'yaw': round(random.uniform(0,360),2),
		'pitch': round(random.uniform(0,99),2),
		'roll': round(random.uniform(0,99),2),
		'lat': round(random.uniform(0,180),2),
		'lon': round(random.uniform(0,360),2),
		'x_velocity': round(random.uniform(0,20),2),
		'y_velocity': round(random.uniform(0,20),2),
		'z_velocity': round(random.uniform(0,20),2),
		'spare': round(random.uniform(0,99),2),
		})
	def ElecCom_s_write(self):
		self.ElecCom_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'head': round(random.uniform(0,360),2),
		'spare': round(random.uniform(0,99),2),
		})
	def GPS_s_write(self):
		self.GPS_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'SIG': round(random.uniform(0,99),2),
		'HDOP': round(random.uniform(0,99),2),
		'lon': round(random.uniform(0,360),2),
		'lat': round(random.uniform(0,180),2),
		'ELV': round(random.uniform(0,99),2),
		'speed': round(random.uniform(0,20),2),
		'course': round(random.uniform(0,360),2),
		'spare': round(random.uniform(0,99),2),
		})
	def Log_s_write(self):
		self.Log_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'lon_water_vel': round(random.uniform(0,30),2),
		'lat_water_vel': round(random.uniform(0,30),2),
		'lon_ground_vel': round(random.uniform(0,30),2),
		'lat_ground_vel': round(random.uniform(0,30),2),
		'stern_water_vel': round(random.uniform(0,30),2),
		'stern_ground_vel': round(random.uniform(0,30),2),
		'mileage_water': round(random.uniform(0,9999),2),
		'zero_mileage_water': round(random.uniform(0,9999),2),
		'mileage_ground': round(random.uniform(0,9999),2),
		'zero_mileage_ground': round(random.uniform(0,9999),2),
		'spare': round(random.uniform(0,99),2),
		})

		#Meteorological&Hydrologic
	def Ane_s_write(self):
		self.Ane_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'rel_speed': round(random.uniform(0,20),2),
		'true_speed': round(random.uniform(0,20),2),
		'rel_direction': round(random.uniform(0,360),2),
		'true _direction': round(random.uniform(0,360),2),
		'height': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def Depth_s_write(self):
		self.Depth_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'sea_depth': round(random.uniform(0,99),2),
		'draft': round(random.uniform(0,99),2),
		'range': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})

		#Perception
	def AIS_s_write(self):
		self.AIS_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'mmsi': str(random.randint(100000000,999999999)),
		'imo': str(random.randint(100000000,999999999)),
		'rate': round(random.uniform(0,99),2),
		'velocity': round(random.uniform(0,20),2),
		'accuracy': round(random.uniform(0,99),2),
		'lon': round(random.uniform(0,360),2),
		'lat': round(random.uniform(0,180),2),
		'rel_head': round(random.uniform(0,360),2),
		'head': round(random.uniform(0,360),2),
		'spare': round(random.uniform(0,360),2),
		})
	def XRdr_s_write(self):
		self.XRdr_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'length_to_ship': round(random.uniform(0,99),2),
		'rel_azimuth': round(random.uniform(0,99),2),
		'azimuth': round(random.uniform(0,99),2),
		'velocity': round(random.uniform(0,99),2),
		'rel_head': round(random.uniform(0,99),2),
		'head': round(random.uniform(0,99),2),
		'meet_length': round(random.uniform(0,99),2),
		'meet_time': round(random.uniform(0,99),2),
		'name': str(random.randint(1000000,9999999)),
		'state': str(random.randint(1000000,9999999)),
		'reference': str(random.randint(1000000,9999999)),
		'spare': round(random.uniform(0,99),2),
		})

		#AuxiliaryMachinery
	def Pump_s_write(self):
		self.Pump_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'ballast_pump_inlet_press': round(random.uniform(0,99),2),
		'ballast_pump_outlet_press': round(random.uniform(0,99),2),
		'fire_pump_inlet_press': round(random.uniform(0,99),2),
		'fire_pump_outlet_press': round(random.uniform(0,99),2),
		'bilge_pump_inlet_press': round(random.uniform(0,99),2),
		'bilge_pump_outlet_press': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def Pump_c_BallastStart_write(self):
		self.Pump_c_BallastStart_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'start': bool(random.randint(0,1)),
		})
	def Pump_c_BallastStop_write(self):
		self.Pump_c_BallastStop_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'stop': bool(random.randint(0,1)),
		})
	def Pump_c_FireStart_write(self):
		self.Pump_c_FireStart_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'start': bool(random.randint(0,1)),
		})
	def Pump_c_FireStop_write(self):
		self.Pump_c_FireStop_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'stop': bool(random.randint(0,1)),
		})
	def Pump_c_BilgeStart_write(self):
		self.Pump_c_BilgeStart_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'start': bool(random.randint(0,1)),
		})
	def Pump_c_BilgeStop_write(self):
		self.Pump_c_BilgeStop_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'stop': bool(random.randint(0,1)),
		})
	def Pump_c_Spare_write(self):
		self.Pump_c_Spare_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': round(random.uniform(0,99),2),
		})
	def Valve_s_write(self):
		self.Valve_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'ballast_valve_opening': round(random.uniform(0,99),2),
		'bilge_valve_opening': round(random.uniform(0,99),2),
		'spare': round(random.uniform(0,99),2),
		})
	def Valve_c_BallastVO_write(self):
		self.Valve_c_BallastVO_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'open': bool(random.randint(0,1)),
		})
	def Valve_c_BallastVC_write(self):
		self.Valve_c_BallastVC_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'close': bool(random.randint(0,1)),
		})
	def Valve_c_BilgeVO_write(self):
		self.Valve_c_BilgeVO_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'open': bool(random.randint(0,1)),
		})
	def Valve_c_BilgeVC_write(self):
		self.Valve_c_BilgeVC_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'close': bool(random.randint(0,1)),
		})
	def Valve_c_Spare_write(self):
		self.Valve_c_Spare_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': round(random.uniform(0,99),2),
		})
	def OtherAuxMach_a_write(self):
		self.OtherAuxMach_a_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'WTDIntegratedAlarm': bool(random.randint(0,1)),
		'WTDOpenning': bool(random.randint(0,1)),
		'WTDClosing': bool(random.randint(0,1)),
		'SideThrIntegrated': bool(random.randint(0,1)),
		'SideThrMotorRunning': bool(random.randint(0,1)),
		'BoadcastPowerLoss': bool(random.randint(0,1)),
		'FireAlarmControllerPowerFaulty': bool(random.randint(0,1)),
		'FireAlarmControllerSystemFaulty': bool(random.randint(0,1)),
		'ACIntegratedAlarm': bool(random.randint(0,1)),
		'CO2Integrated': bool(random.randint(0,1)),
		'CO2Leakage': bool(random.randint(0,1)),
		'CO2Discharge': bool(random.randint(0,1)),
		'ChargePlateChargerFault': bool(random.randint(0,1)),
		'ChargePlateLowInsulation': bool(random.randint(0,1)),
		'ChargePlateBatteryDischarge': bool(random.randint(0,1)),
		'ChargePlateBatteryVoltage': bool(random.randint(0,1)),
		'ChargePlateIntegratedAlarm': bool(random.randint(0,1)),
		'UltravioletDisinfectionIntegrated': bool(random.randint(0,1)),
		'HighOilConcentrationAlarm': bool(random.randint(0,1)),
		'ControllerIntegratedAlarm': bool(random.randint(0,1)),
		'AlarmLampRelayBoxMainPowerLoss': bool(random.randint(0,1)),
		'AlarmLampRelayBoxStandbyPowerLoss': bool(random.randint(0,1)),
		'FireDamperControlBoxPowerFault': bool(random.randint(0,1)),
		'ElectrolyticAnticorrosiveCtrlBox1Run': bool(random.randint(0,1)),
		'ElectrolyticAnticorrosiveCtrlBox1Alarm': bool(random.randint(0,1)),
		'ElectrolyticAntifoulingCtrlBox2Run': bool(random.randint(0,1)),
		'ElectrolyticAntifoulingCtrlBox2Alarm': bool(random.randint(0,1)),
		'spare': bool(random.randint(0,1)),
		})

		#Intelligent
	def IePept_PeptFus_s_write(self):
		self.IePept_PeptFus_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'lon': round(random.uniform(0,360),2),
		'lat': round(random.uniform(0,180),2),
		'category': round(random.uniform(0,9),2),
		'absolute_bearing': round(random.uniform(0,360),2),
		'speed': round(random.uniform(0,99),2),
		'head': round(random.uniform(0,360),2),
		'accuracy': round(random.uniform(0,99),2),
		'dimension': round(random.uniform(0,9999),2),
		'spare': round(random.uniform(0,99),2),
		})
	def IeCtrl_Pilot_c_Angle_write(self):
		self.IeCtrl_Pilot_c_Angle_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'angle': round(random.uniform(0,70),2),
		})
	def IeCtrl_Pilot_c_Direction_write(self):
		self.IeCtrl_Pilot_c_Direction_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'direction': 'L/R',
		})
	def IeCtrl_Pilot_c_Spare_write(self):
		self.IeCtrl_Pilot_c_Spare_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': round(random.uniform(0,99),2),
		})
	def IeCtrl_PropMot_c_Speed_write(self):
		self.IeCtrl_PropMot_c_Speed_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'speed': round(random.uniform(0,1000),2),
		})
	def IeCtrl_PropMot_c_Torque_write(self):
		self.IeCtrl_PropMot_c_Torque_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'torque': round(random.uniform(0,3000),2),
		})
	def IeCtrl_PropMot_c_SftPower_write(self):
		self.IeCtrl_PropMot_c_SftPower_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'power': round(random.uniform(0,3000),2),
		})
	def IeCtrl_PropMot_c_Spare_write(self):
		self.IeCtrl_PropMot_c_Spare_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': round(random.uniform(0,99),2),
		})
	def IeCtrl_ST_c_Speed_write(self):
		self.IeCtrl_ST_c_Speed_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'speed': round(random.uniform(0,1000),2),
		})
	def IeCtrl_ST_c_Torque_write(self):
		self.IeCtrl_ST_c_Torque_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'torque': round(random.uniform(0,1000),2),
		})
	def IeCtrl_ST_c_SftPower_write(self):
		self.IeCtrl_ST_c_SftPower_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'power': round(random.uniform(0,3000),2),
		})
	def IeCtrl_ST_c_Spare_write(self):
		self.IeCtrl_ST_c_Spare_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'spare': round(random.uniform(0,99),2),
		})
	def IeCtrl_RT_s_write(self):
		self.IeCtrl_RT_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'x_axis_err': round(random.uniform(0,99),2),
		'y_axis_err': round(random.uniform(0,99),2),
		'set_heading': round(random.uniform(0,99),2),
		'set_speed': round(random.uniform(0,99),2),
		'target_distance': round(random.uniform(0,99),2),
		'acceleration': round(random.uniform(0,99),2),
		'mode': random.randint(1,20),
		'spare': round(random.uniform(0,99),2),
		})
	def IeDec_RP_s_write(self):
		self.IeDec_RP_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'waypoint': [156,484,651,61,6,4168],
		'spare': round(random.uniform(0,99),2),
		})
	def IeDec_NaviInfo_s_write(self):
		self.IeDec_NaviInfo_s_ins.data = json.dumps({ 
		'time': time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()),
		'lon': round(random.uniform(0,360),2),
		'lat': round(random.uniform(0,180),2),
		'head': round(random.uniform(0,360),2),
		'tgt_rel_head': round(random.uniform(0,360),2),
		'tgt_head': round(random.uniform(0,360),2),
		'spare': round(random.uniform(0,99),2),
		})


def main(args=None):
	rclpy.init(args=args)
	ros2_msg_pub = PubMsg()
	rclpy.spin(ros2_msg_pub)
	ros2_msg_pub.destroy_node()
	rclpy.shutdown()

if __name__ == '__main__':
	main()
