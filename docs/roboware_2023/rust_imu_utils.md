# rust_imu_utils
IMUに関わる操作を行うためのライブラリです。


## How to use
Cargo.tomlに以下を記述する
```toml
[dependencies]
rust_imu_utils = {git = "https://github.com/motii8128/rust_imu_utils.git"}
```

## Functions

#### xyz_to_quaternion
オイラー角をクォータニオンに変換する関数です。
```rust
pub fn xyz_to_quaternion(euler:na::Vector3<f32>)->na::Quaternion<f32>
```


#### convert_to_vector
３つのf32の変数を含んだ、nalgebra用の３次元ベクトルを生成します。
このライブラリのEKFではnalgebraのVectorを引数にするのでこちらで変換してください。
```rust
pub fn convert_to_vector(x:f32, y:f32, z:f32)->na::Vector3<f32>
```

#### remove_gravity_from_euler
一定の重力、そしてオイラー角の姿勢を使って加速度の重力を除去する関数です。
x, y, zのタプルを返します。
```rust
pub fn remove_gravity_from_euler(linear_accel:na::Vector3<f32>,euler:na::Vector3<f32>, gravity:f32)->(f32, f32, f32)
```



## EKF for Posture
加速度センサー、角速度センサーの値から姿勢を推定する

***Initialize***
```rust
use rust_imu_utils::ekf::Axis6EKF;

let delta_time = 0.001;
let ekf = Axis6EKF::new(delta_time);
```

***in the loop***
```rust
let linear_accel = rust_imu_utils::convert_to_vector(linear_x, linear_y, linear_z);
let gyro_velocity = rust_imu_utils::convert_to_vector(angular_velocity_x, angular_velocity_x, angular_velocity_x);

let result_euler_angle = ekf.run_ekf(gyro_velocity, linear_accel, delta_time);

// result_euler_angle.x：推定したxの角度
// result_euler_angle.y：推定したyの角度
// result_euler_angle.z：推定したzの角度

std::thread::sleep(std::time::Duration::from_millis(1));
```

ROS2で実装したもの

[posture_estimater](https://github.com/motii8128/posture_estimater.git)