## Table of content

[1. Hooks](#1-hooks)   
[2. Kafka](#2-kafka)  
[3. Elasticsearch](#3-elasticsearch)  
[4. Logstash](#3-logstash)   
[5. Kibana](#3-kibana)    
[6. Prometheus](#3-prometheus)      
[7. Grafana](#3-grafana) 


### 1. Hooks

<details>
<summary><b>useRef</b></summary>

</details>

<details>
<summary><b>useState</b></summary>

```tsx
const [state, setState] = useState(initialValue);
```

- Có thể set trực tiếp `state` bằng: `state: ‘abc’`, nhưng khi đó component sẽ không re-render, component chỉ re-render khi set state bằnh hàm `setState(value)`
- Hàm `setState` là hàm bất đồng bộ, khi gọi hàm `setState`:
    - Component sẽ không được re-render ngay lập tức, mà React sẽ đánh dấu component đó cần re-render.
    - Giá trị `state` vẫn chưa được cập nhật ngay trong lần render hiện tại.
    - React dùng cơ chế batch update: gom nhiều setState trong cùng một sự kiện lại rồi thực hiện re-render một lần duy nhất, để tối ưu hiệu suất.
- hàm setState có thể nhận vào 1 giá trị hoặc 1 callback (function updater). Dùng function updater khi giá trị cần setState có liên quan đến giá trị trước của state
    - Khi gọi `setState(state + 1)` 3 lần liên tiếp thì state chỉ tăng lên 1 đơn vị ⇒ dùng `setState(state => state + 1)`

</details>


<details>
<summary><b>useEffect</b></summary>

- dependency:
    - Nếu không truyền: callback sẽ chạy lại mỗi khi component bị re-render (do sự thay đổi có props/state/do component cha re-render, v,v….)
    - Truyền mảng rỗng: sẽ chạy duy nhất khi component được render lần đầu tiên
    - Nếu dependency có tham số thì callback sẽ chạy lại mỗi khi tham số đó bị thay đổi (React dùng cơ chế **Shallow comparison** để so sánh sự thay đổi)
- Cleanup trong useEffect:

    ```tsx
    useEffect(() => {
        const timer = setInterval(() => { ... }, 1000);

        return () => clearInterval(timer); // cleanup
    }, []);
    ```
    Hàm này hoạt động như componentWillUnmount, dùng để reset các trạng thái, các timer, các interval được khai báo trong component

- Không thể khai báo trực tiếp async cho callback của useEffect:

    ```tsx
    useEffect(async () => {}, []) // ❌ Sai
    ```

- mà phải là
    ```tsx
    useEffect(() => {
        const fetchData = async () => { ... };

        fetchData();
    }, []);
    ```
</details>


<details>
<summary><b>useLayoutEffect</b></summary>

- useLayoutEffect khác useEffect ở chỗ là: nó sẽ chạy đồng bộ ngay từ trước khi component được render. Còn useEffect sẽ chạy sau khi component được mounting (nếu trong callback của useEffect có cập nhật state thì component sẽ bị re-render lại sau đó)
- useLayoutEffect thường dùng khi muốn thao tác với DOM ngay trước khi component được render (setValue, setSize, hiển thị sẵn giao diện, v.v…..)

</details>
<details>
<summary><b>useMemo</b></summary>

useMemo là lưu kết quả của hàm (giá trị), còn useCallback là lưu cả hàm (callback). Ví dụ: dùng useMemo để lưu lại kết quả của 1 hàm tính toán phức tạp dựa vào 1 biến: dependency nào đó; dùng useCallback khi muốn truyền 1 hàm nào đó vào component khác dưới dạng props
</details>

<details>
<summary><b>useCallback</b></summary>

</details>
<details>
<summary><b>useContext</b></summary>

</details>
<details>
<summary><b>useReducer</b></summary>

</details>

