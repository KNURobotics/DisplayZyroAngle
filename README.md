# 자이로센서로 xyz축 각도 계측
``` C
#include<Wire.h>

const int MPU = 0x68;  //MPU 6050 의 I2C 기본 주소
int16_t AcX,AcY,AcZ, Tmp, GyX,GyY,GyZ;

void setup(){

  Wire.begin();      //Wire 라이브러리 초기화
  Wire.beginTransmission(MPU); //MPU로 데이터 전송 시작
  Wire.write(0x6B);  // PWR_MGMT_1 register
  Wire.write(0);     //MPU-6050 시작 모드로
  Wire.endTransmission(true);

  Serial.begin(9600);

}

void loop(){
  Wire.beginTransmission(MPU);    //데이터 전송시작
  Wire.write(0x3B);               // register 0x3B (ACCEL_XOUT_H), 큐에 데이터 기록
  Wire.endTransmission(false);    //연결유지

  Wire.requestFrom(MPU,6,true);  //MPU에 데이터 요청


  //데이터 한 바이트 씩 읽어서 반환
  AcX = Wire.read() << 8 | Wire.read();  // 0x3B (ACCEL_XOUT_H) & 0x3C (ACCEL_XOUT_L)    
  AcY = Wire.read() << 8 | Wire.read();  // 0x3D (ACCEL_YOUT_H) & 0x3E (ACCEL_YOUT_L)
  AcZ = Wire.read() << 8 | Wire.read();  // 0x3F (ACCEL_ZOUT_H) & 0x40 (ACCEL_ZOUT_L)
  //Tmp = Wire.read() << 8 | Wire.read();  // 0x41 (TEMP_OUT_H) & 0x42 (TEMP_OUT_L)
  //GyX = Wire.read() << 8 | Wire.read();  // 0x43 (GYRO_XOUT_H) & 0x44 (GYRO_XOUT_L)
  //GyY = Wire.read() << 8 | Wire.read();  // 0x45 (GYRO_YOUT_H) & 0x46 (GYRO_YOUT_L)
  //GyZ = Wire.read() << 8 | Wire.read();  // 0x47 (GYRO_ZOUT_H) & 0x48 (GYRO_ZOUT_L)


  float RADIAN_TO_DEGREES = 180/3.14139;
  float val_y = atan(AcX/sqrt(pow(AcY,2) + pow(AcZ,2))) * RADIAN_TO_DEGREES;
  float val_x = atan(AcY/sqrt(pow(AcX,2) + pow(AcZ,2))) * RADIAN_TO_DEGREES;
  float val_z = atan(AcX/sqrt(pow(AcZ,2) + pow(AcY,2))) * RADIAN_TO_DEGREES;

  //시리얼 모니터에 출력
  //Serial.print("AcX = ");
  Serial.print("x = ");
  Serial.println(val_x);
  Serial.print("y = ");
  Serial.println(val_y);
  Serial.print("z = ");
  Serial.println(val_z);
  //Serial.print(" | AcY = "); Serial.println(val_x);
  //Serial.print(" | AcZ = "); Serial.println(AcZ);
  //Serial.print(" | Tmp = "); Serial.print(Tmp/340.00+36.53);  
  //Serial.print(" | GyX = "); Serial.print(GyX);
  //Serial.print(" | GyY = "); Serial.print(GyY);
  //Serial.print(" | GyZ = "); Serial.println(GyZ);
  delay(200);
}
```



[가속도 센서를 이용해 각도값 출력](http://blog.naver.com/PostView.nhn?blogId=elrlemrm&logNo=220312173533)
