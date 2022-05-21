# Transaction
import React, { Component } from "react";
import { View, Text, StyleSheet, TouchableOpacity } from "react-native";
import * as Permissions from "expo-permissions";
import { BarCodeScanner } from "expo-barcode-scanner";
import { TextInput } from "react-native-gesture-handler";
import { CAMERA } from "expo-permissions";

export default class TransactionScreen extends Component {
  constructor(props) {
    super(props);
    this.state = {
      domState: "normal",
      hasCameraPermissions: null,
      scanned: false,
      scannedData: "",
      bookId:"",
      studentId:""
    };
  }

  getCameraPermissions = async domState => {
    const { status } = await Permissions.askAsync(Permissions.CAMERA);

    this.setState({
      /*status === "granted" is true when user has granted permission
          status === "granted" is false when user has not granted the permission
        */
      hasCameraPermissions: status === "granted",
      domState: domState,
      scanned: false
    });
  };

  handleBarCodeScanned = async ({ type, data }) => {
    this.setState({
      scannedData: data,
      domState: "normal",
      scanned: true
    });
  };
  handleBarCodeScanned = async({ type, data }) => { const { domState } = this.state; 
   if (domState === "bookId") 
   { this.setState({ bookId: data, domState: "normal", scanned: true }); } 
   else if (domState === "studentId") 
   { this.setState({ studentId: data, domState: "normal", scanned: true }); } }
  render() {
    const { domState, hasCameraPermissions, scannedData, scanned,bookId,studentId } = this.state;

  if (domState !== "normal") { 
    return ( <BarCodeScanner onBarCodeScanned={scanned ? undefined : this.handleBarCodeScanned} 
      style={StyleSheet.absoluteFillObject} /> ); }



 return( 
  <View style={styles.container}> 
  <View style={styles.lowerContainer}>
    <View style={styles.textInputContainer}>
    <TextInput style={styles.textinput} 
    placeholder={"Book Id"} placeholderTextColor={"#FFFFFF"} value={bookId} />
     <TouchableOpacity style={styles.scanbutton} onPress={() => this.getCameraPermissions("bookId")} > 
     <Text style={styles.scanbuttonText}>Scan</Text>
      </TouchableOpacity>
    </View>
    </View> 
  </View>) 



}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: "#5653D4"
  },
  text: {
    color: "#ffff",
    fontSize: 15
  },
  button: {
    width: "43%",
    height: 55,
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: "#F48D20",
    borderRadius: 15
  },
  buttonText: {
    fontSize: 24,
    color: "#FFFFFF"
  },
  lowerContainer:{flex:0.5,alignItems:'center'},
  textInputContainer:{borderWidth:2, borderRadius:10, 
    flexDirection:"row", 
    backgroundColor:'#9DFD24',
    borderColor:'white'
},
  textinput:{
    width:'57%',
    height:50,
    padding:10,
    borderColor:'white',
    borderRadius:10,
    borderWidth:3,
    fontSize:18,
    backgroundColor:'#5653d4',
    fontFamily:"Rajdhani_600SemiBold",
    color:'white'

  },
  scanbutton:{
      width:100,
      height:50,
      backgroundColor:'#9DFD24',
      borderTopRightRadius:10,
      borderBottomRightRadius:10,
      justifyContent:'center',
      alignItems:'center',

  },
  scanbuttonText:{
      fontSize:24,
      color:"#0A0101",
      fontFamily:'RajDhani_600SemiBold'
  },
  bgImage:{flex:1, resizeMode:'cover',justifyContent:"center"},
  upperContainer:{flex:0.5, alignItems:"center", justifyContent:"center"},
  appIcon:{width:200,height:200, resizeMode:"contain",marginTop:80},
  appName:{width:80,height:80, resizeMode:"contain"},
})
