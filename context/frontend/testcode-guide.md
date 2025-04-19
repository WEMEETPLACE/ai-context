# TEST CODE 가이드

이 문서는 프론트엔드 개발에서 테스트하기 좋은 코드를 작성하기 위한 주요 원칙들을 설명합니다. 각 원칙에는 좋은 예와 나쁜 예가 포함되어 있습니다.

## 목차

1. [컴포넌트 책임 분리](#1-컴포넌트-책임-분리)
2. [의존성 주입 (DI)](#2-의존성-주입-di)
3. [순수 함수 (Pure Functions)](#3-순수-함수-pure-functions)
4. [작은 단위의 컴포넌트와 함수](#4-작은-단위의-컴포넌트와-함수)
5. [커스텀 훅으로 로직 분리](#5-커스텀-훅으로-로직-분리)
6. [상태 관리 단순화](#6-상태-관리-단순화)
7. [비동기 처리 테스트 용이성](#7-비동기-처리-테스트-용이성)
8. [사이드 이펙트 격리](#8-사이드-이펙트-격리)
9. [이벤트 핸들러 테스트 용이성](#9-이벤트-핸들러-테스트-용이성)
10. [Props 및 상태 검증](#10-props-및-상태-검증)
11. [접근성 및 UI 컴포넌트 테스트](#11-접근성-및-ui-컴포넌트-테스트)

## 1. 컴포넌트 책임 분리

**핵심 원칙**: 각 컴포넌트는 단 하나의 책임만 가지도록 설계합니다.

### 좋은 예

```jsx
// UserProfile: 사용자 프로필 표시만 담당
const UserProfile = ({ user }) => (
  <div>
    <h2>{user.name}</h2>
    <p>Email: {user.email}</p>
    <p>Role: {user.role}</p>
  </div>
);

// UserActions: 사용자 관련 액션 담당
const UserActions = ({ onEdit, onDelete }) => (
  <div>
    <button onClick={onEdit}>Edit Profile</button>
    <button onClick={onDelete}>Delete Account</button>
  </div>
);

// UserCard: 하위 컴포넌트 조합
const UserCard = ({ user, onEdit, onDelete }) => (
  <div className="user-card">
    <UserProfile user={user} />
    <UserActions onEdit={onEdit} onDelete={onDelete} />
  </div>
);
```

### 나쁜 예

```jsx
// 너무 많은 책임: 데이터 표시, 액션 처리, 스타일링 등을 모두 처리
const UserCard = ({ userId }) => {
  const [user, setUser] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // 데이터 fetching 로직
    fetchUser(userId)
      .then((data) => {
        setUser(data);
        setIsLoading(false);
      })
      .catch((err) => {
        setError(err);
        setIsLoading(false);
      });
  }, [userId]);

  const handleEdit = () => {
    // 편집 로직
  };

  const handleDelete = () => {
    // 삭제 로직
    deleteUser(userId)
      .then(() => {
        alert("User deleted");
      })
      .catch((err) => {
        alert(`Error: ${err.message}`);
      });
  };

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  if (!user) return null;

  return (
    <div className="user-card">
      <h2>{user.name}</h2>
      <p>Email: {user.email}</p>
      <p>Role: {user.role}</p>
      <button onClick={handleEdit}>Edit Profile</button>
      <button onClick={handleDelete}>Delete Account</button>
    </div>
  );
};
```

## 2. 의존성 주입 (DI)

**핵심 원칙**: 컴포넌트나 훅이 필요한 의존성을 직접 생성하지 않고 외부에서 받도록 설계합니다.

### 좋은 예

```jsx
// 의존성을 외부에서 주입받음
const useUserData = (fetchClient = defaultFetchClient) => {
  const [user, setUser] = useState(null);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const fetchUser = async (id) => {
    setIsLoading(true);
    try {
      const data = await fetchClient.get(`/users/${id}`);
      setUser(data);
      setError(null);
    } catch (err) {
      setError(err);
      setUser(null);
    } finally {
      setIsLoading(false);
    }
  };

  return { user, isLoading, error, fetchUser };
};

// 테스트 코드
test("fetches user correctly", async () => {
  const mockClient = {
    get: jest.fn().mockResolvedValue({ id: "123", name: "Test User" }),
  };

  const { result } = renderHook(() => useUserData(mockClient));

  await act(async () => {
    await result.current.fetchUser("123");
  });

  expect(mockClient.get).toHaveBeenCalledWith("/users/123");
  expect(result.current.user).toEqual({ id: "123", name: "Test User" });
  expect(result.current.isLoading).toBe(false);
  expect(result.current.error).toBe(null);
});
```

### 나쁜 예

```jsx
// 내부에서 의존성 직접 생성 - 테스트하기 어려움
const useUserData = () => {
  const [user, setUser] = useState(null);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  // axios 같은 특정 HTTP 클라이언트에 직접 의존
  const fetchUser = async (id) => {
    setIsLoading(true);
    try {
      const response = await axios.get(`/users/${id}`);
      setUser(response.data);
      setError(null);
    } catch (err) {
      setError(err);
      setUser(null);
    } finally {
      setIsLoading(false);
    }
  };

  return { user, isLoading, error, fetchUser };
};

// 테스트가 어려움 - axios를 모킹해야 함
```

## 3. 순수 함수 (Pure Functions)

**핵심 원칙**: 동일한 입력에 항상 동일한 출력을 반환하고 부작용이 없는 함수를 작성합니다.

### 좋은 예

```jsx
// 순수 함수: 같은 입력에 항상 같은 출력, 부작용 없음
const calculateTotalPrice = (items) =>
  items.reduce((sum, item) => sum + item.price * item.quantity, 0);

const formatCurrency = (amount) => `$${amount.toFixed(2)}`;

// 컴포넌트에서 활용
const ShoppingCartSummary = ({ items }) => {
  const totalPrice = calculateTotalPrice(items);

  return (
    <div>
      <h3>Cart Summary</h3>
      <p>Total Items: {items.length}</p>
      <p>Total Price: {formatCurrency(totalPrice)}</p>
    </div>
  );
};

// 테스트하기 쉬움
test("calculateTotalPrice calculates correctly", () => {
  const items = [
    { id: 1, name: "Item 1", price: 10, quantity: 2 },
    { id: 2, name: "Item 2", price: 15, quantity: 1 },
  ];
  expect(calculateTotalPrice(items)).toBe(35);
});

test("formatCurrency formats correctly", () => {
  expect(formatCurrency(10)).toBe("$10.00");
  expect(formatCurrency(10.5)).toBe("$10.50");
});
```

### 나쁜 예

```jsx
// 전역 상태에 의존하는 비순수 함수
let taxRate = 0.1;
let discountAmount = 0;

const calculateTotalWithTaxAndDiscount = (items) => {
  const subtotal = items.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );
  // 전역 변수에 의존하므로 테스트가 어려움
  return (subtotal - discountAmount) * (1 + taxRate);
};

// 사이드 이펙트가 있는 함수
const applyDiscount = (code) => {
  if (code === "SALE20") {
    discountAmount = 20; // 전역 상태 변경
    return true;
  }
  return false;
};

// 테스트 시, 전역 변수를 조작해야 하므로 테스트 간 간섭 가능성
```

## 4. 작은 단위의 컴포넌트와 함수

**핵심 원칙**: 한 가지 일만 하는 작은 컴포넌트와 함수로 코드를 분리합니다.

### 좋은 예

```jsx
// 작은 단위 컴포넌트들
const FormField = ({ label, error, children }) => (
  <div className="form-field">
    <label>{label}</label>
    {children}
    {error && <span className="error">{error}</span>}
  </div>
);

const EmailField = ({ value, onChange, error }) => (
  <FormField label="Email" error={error}>
    <input
      type="email"
      value={value}
      onChange={onChange}
      className={error ? "input-error" : ""}
    />
  </FormField>
);

const PasswordField = ({ value, onChange, error }) => (
  <FormField label="Password" error={error}>
    <input
      type="password"
      value={value}
      onChange={onChange}
      className={error ? "input-error" : ""}
    />
  </FormField>
);

// 작은 단위 유틸리티 함수들
const validateEmail = (email) => {
  if (!email) return "Email is required";
  if (!/\S+@\S+\.\S+/.test(email)) return "Email is invalid";
  return null;
};

const validatePassword = (password) => {
  if (!password) return "Password is required";
  if (password.length < 8) return "Password must be at least 8 characters";
  return null;
};

// 컴포넌트 조합
const LoginForm = () => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const emailError = validateEmail(email);
  const passwordError = validatePassword(password);
  const isValid = !emailError && !passwordError;

  return (
    <form>
      <EmailField
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        error={emailError}
      />
      <PasswordField
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        error={passwordError}
      />
      <button disabled={!isValid} type="submit">
        Login
      </button>
    </form>
  );
};
```

### 나쁜 예

```jsx
// 너무 큰 컴포넌트
const LoginForm = () => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [emailError, setEmailError] = useState(null);
  const [passwordError, setPasswordError] = useState(null);

  const validateEmail = () => {
    if (!email) {
      setEmailError("Email is required");
      return false;
    }
    if (!/\S+@\S+\.\S+/.test(email)) {
      setEmailError("Email is invalid");
      return false;
    }
    setEmailError(null);
    return true;
  };

  const validatePassword = () => {
    if (!password) {
      setPasswordError("Password is required");
      return false;
    }
    if (password.length < 8) {
      setPasswordError("Password must be at least 8 characters");
      return false;
    }
    setPasswordError(null);
    return true;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const isEmailValid = validateEmail();
    const isPasswordValid = validatePassword();

    if (isEmailValid && isPasswordValid) {
      // 로그인 처리
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div className="form-field">
        <label>Email</label>
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          className={emailError ? "input-error" : ""}
        />
        {emailError && <span className="error">{emailError}</span>}
      </div>

      <div className="form-field">
        <label>Password</label>
        <input
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          className={passwordError ? "input-error" : ""}
        />
        {passwordError && <span className="error">{passwordError}</span>}
      </div>

      <button type="submit">Login</button>
    </form>
  );
};
```

## 5. 커스텀 훅으로 로직 분리

**핵심 원칙**: 컴포넌트의 상태 관리와 로직을 커스텀 훅으로 분리하여 테스트 가능성을 높입니다.

### 좋은 예

```jsx
// 폼 로직을 커스텀 훅으로 분리
const useLoginForm = (onSubmit) => {
  const [values, setValues] = useState({ email: "", password: "" });
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues({ ...values, [name]: value });
  };

  const validate = () => {
    const newErrors = {};
    if (!values.email) newErrors.email = "Email is required";
    else if (!/\S+@\S+\.\S+/.test(values.email))
      newErrors.email = "Email is invalid";

    if (!values.password) newErrors.password = "Password is required";
    else if (values.password.length < 8)
      newErrors.password = "Password must be at least 8 characters";

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (validate()) {
      setIsSubmitting(true);
      try {
        await onSubmit(values);
      } catch (error) {
        setErrors({ form: error.message });
      } finally {
        setIsSubmitting(false);
      }
    }
  };

  return {
    values,
    errors,
    isSubmitting,
    handleChange,
    handleSubmit,
  };
};

// 컴포넌트는 UI 렌더링에만 집중
const LoginForm = ({ onLogin }) => {
  const { values, errors, isSubmitting, handleChange, handleSubmit } =
    useLoginForm(onLogin);

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Email</label>
        <input
          type="email"
          name="email"
          value={values.email}
          onChange={handleChange}
        />
        {errors.email && <p>{errors.email}</p>}
      </div>

      <div>
        <label>Password</label>
        <input
          type="password"
          name="password"
          value={values.password}
          onChange={handleChange}
        />
        {errors.password && <p>{errors.password}</p>}
      </div>

      {errors.form && <p>{errors.form}</p>}

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Logging in..." : "Login"}
      </button>
    </form>
  );
};

// 커스텀 훅 테스트
test("useLoginForm validates email correctly", () => {
  const handleSubmit = jest.fn();
  const { result } = renderHook(() => useLoginForm(handleSubmit));

  // 잘못된 이메일 제출
  act(() => {
    result.current.values.email = "invalid-email";
    result.current.handleSubmit({ preventDefault: jest.fn() });
  });

  expect(result.current.errors.email).toBe("Email is invalid");
  expect(handleSubmit).not.toHaveBeenCalled();
});
```

### 나쁜 예

```jsx
// 로직과 UI가 혼합된 컴포넌트
const LoginForm = ({ onLogin }) => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleEmailChange = (e) => setEmail(e.target.value);
  const handlePasswordChange = (e) => setPassword(e.target.value);

  const handleSubmit = async (e) => {
    e.preventDefault();
    const newErrors = {};

    if (!email) newErrors.email = "Email is required";
    else if (!/\S+@\S+\.\S+/.test(email)) newErrors.email = "Email is invalid";

    if (!password) newErrors.password = "Password is required";
    else if (password.length < 8)
      newErrors.password = "Password must be at least 8 characters";

    setErrors(newErrors);

    if (Object.keys(newErrors).length === 0) {
      setIsSubmitting(true);
      try {
        await onLogin({ email, password });
      } catch (error) {
        setErrors({ form: error.message });
      } finally {
        setIsSubmitting(false);
      }
    }
  };

  return <form onSubmit={handleSubmit}>{/* 폼 UI */}</form>;
};

// 컴포넌트 상태와 로직이 혼합되어 테스트하기 어려움
```

## 6. 상태 관리 단순화

**핵심 원칙**: 상태 변화를 예측 가능하고 테스트하기 쉽게 만듭니다.

### 좋은 예

```jsx
// Zustand 스토어 - 예측 가능한 상태 변화
import { create } from "zustand";

interface CartState {
  items: CartItem[];
  addItem: (item: CartItem) => void;
  removeItem: (itemId: string) => void;
  updateQuantity: (itemId: string, quantity: number) => void;
  clearCart: () => void;
}

interface CartItem {
  id: string;
  name: string;
  price: number;
  quantity: number;
}

const useCartStore =
  create <
  CartState >
  ((set) => ({
    items: [],

    addItem: (item) =>
      set((state) => {
        const existingItem = state.items.find((i) => i.id === item.id);
        if (existingItem) {
          return {
            items: state.items.map((i) =>
              i.id === item.id ? { ...i, quantity: i.quantity + 1 } : i
            ),
          };
        }
        return { items: [...state.items, { ...item, quantity: 1 }] };
      }),

    removeItem: (itemId) =>
      set((state) => ({
        items: state.items.filter((item) => item.id !== itemId),
      })),

    updateQuantity: (itemId, quantity) =>
      set((state) => ({
        items: state.items.map((item) =>
          item.id === itemId ? { ...item, quantity } : item
        ),
      })),

    clearCart: () => set({ items: [] }),
  }));

// 컴포넌트
const ShoppingCart = () => {
  const { items, removeItem, updateQuantity, clearCart } = useCartStore();
  const totalPrice = items.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );

  return (
    <div className="cart">
      <h2>Shopping Cart</h2>
      {items.length === 0 ? (
        <p>Your cart is empty</p>
      ) : (
        <>
          <ul>
            {items.map((item) => (
              <li key={item.id}>
                {item.name} - ${item.price} x
                <input
                  type="number"
                  min="1"
                  value={item.quantity}
                  onChange={(e) =>
                    updateQuantity(item.id, Number(e.target.value))
                  }
                />
                <button onClick={() => removeItem(item.id)}>Remove</button>
              </li>
            ))}
          </ul>
          <p>Total: ${totalPrice.toFixed(2)}</p>
          <button onClick={clearCart}>Clear Cart</button>
        </>
      )}
    </div>
  );
};

// 테스트
test("cart store adds items correctly", () => {
  const store = useCartStore.getState();

  act(() => {
    store.addItem({ id: "1", name: "Product", price: 10, quantity: 0 });
  });

  expect(useCartStore.getState().items).toEqual([
    { id: "1", name: "Product", price: 10, quantity: 1 },
  ]);

  act(() => {
    store.addItem({ id: "1", name: "Product", price: 10, quantity: 0 });
  });

  expect(useCartStore.getState().items[0].quantity).toBe(2);
});
```

### 나쁜 예

```jsx
// 컴포넌트 내 복잡한 상태 관리
const ShoppingCart = () => {
  const [items, setItems] = useState([]);
  const [error, setError] = useState(null);

  const addItem = (newItem) => {
    const existingItemIndex = items.findIndex((item) => item.id === newItem.id);

    if (existingItemIndex > -1) {
      const updatedItems = [...items];
      updatedItems[existingItemIndex].quantity += 1;
      setItems(updatedItems);
    } else {
      setItems([...items, { ...newItem, quantity: 1 }]);
    }
  };

  const removeItem = (itemId) => {
    setItems(items.filter((item) => item.id !== itemId));
  };

  const updateQuantity = (itemId, quantity) => {
    if (quantity < 1) {
      setError("Quantity must be at least 1");
      return;
    }

    setItems(
      items.map((item) => (item.id === itemId ? { ...item, quantity } : item))
    );
    setError(null);
  };

  const clearCart = () => {
    setItems([]);
  };

  // 컴포넌트 렌더링 로직...
};

// 상태 관리 로직과 UI가 혼합되어 테스트하기 어려움
```

## 7. 비동기 처리 테스트 용이성

**핵심 원칙**: 비동기 코드를 테스트하기 쉽게 구조화합니다.

### 좋은 예

```jsx
// 데이터 fetching 로직을 커스텀 훅으로 분리
const useProductData = (apiClient = defaultApiClient) => {
  const [products, setProducts] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const fetchProducts = useCallback(
    async (category) => {
      setIsLoading(true);
      setError(null);

      try {
        const data = await apiClient.get(`/products?category=${category}`);
        setProducts(data);
      } catch (err) {
        setError(err.message || "Failed to fetch products");
        setProducts([]);
      } finally {
        setIsLoading(false);
      }
    },
    [apiClient]
  );

  return { products, isLoading, error, fetchProducts };
};

// 컴포넌트
const ProductList = ({ category, apiClient }) => {
  const { products, isLoading, error, fetchProducts } =
    useProductData(apiClient);

  useEffect(() => {
    fetchProducts(category);
  }, [category, fetchProducts]);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      <h2>{category} Products</h2>
      <ul>
        {products.map((product) => (
          <li key={product.id}>
            {product.name} - ${product.price}
          </li>
        ))}
      </ul>
    </div>
  );
};

// 테스트
test("useProductData fetches and stores products", async () => {
  const mockProducts = [{ id: 1, name: "Product 1", price: 10 }];
  const mockApiClient = {
    get: jest.fn().mockResolvedValue(mockProducts),
  };

  const { result } = renderHook(() => useProductData(mockApiClient));

  expect(result.current.isLoading).toBe(false);

  await act(async () => {
    await result.current.fetchProducts("electronics");
  });

  expect(mockApiClient.get).toHaveBeenCalledWith(
    "/products?category=electronics"
  );
  expect(result.current.products).toEqual(mockProducts);
  expect(result.current.isLoading).toBe(false);
  expect(result.current.error).toBe(null);
});
```

### 나쁜 예

```jsx
// 컴포넌트 내에 직접 API 호출
const ProductList = ({ category }) => {
  const [products, setProducts] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchProducts = async () => {
      setIsLoading(true);
      setError(null);

      try {
        // 직접 fetch API 사용
        const response = await fetch(`/api/products?category=${category}`);
        if (!response.ok) throw new Error("Failed to fetch");

        const data = await response.json();
        setProducts(data);
      } catch (err) {
        setError(err.message);
        setProducts([]);
      } finally {
        setIsLoading(false);
      }
    };

    fetchProducts();
  }, [category]);

  // 컴포넌트 렌더링...
};

// 전역 fetch API에 의존하므로 테스트하기 어려움
```

## 8. 사이드 이펙트 격리

**핵심 원칙**: 부작용(사이드 이펙트)을 명확하게 분리하고 격리합니다.

### 좋은 예

```jsx
// 순수 비즈니스 로직
const calculateDiscount = (items, couponCode) => {
  const subtotal = items.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );

  switch (couponCode) {
    case "SAVE10":
      return subtotal * 0.1;
    case "SAVE20":
      return subtotal * 0.2;
    case "50OFF":
      return subtotal >= 100 ? 50 : 0;
    default:
      return 0;
  }
};

// 사이드 이펙트가 있는 커스텀 훅
const useCheckout = (calculateDiscountFn = calculateDiscount) => {
  const [items, setItems] = useState([]);
  const [couponCode, setCouponCode] = useState("");
  const [status, setStatus] = useState("idle");

  const subtotal = items.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );
  const discount = calculateDiscountFn(items, couponCode);
  const total = subtotal - discount;

  const applyCoupon = (code) => {
    setCouponCode(code);
  };

  const processCheckout = async (paymentInfo) => {
    setStatus("processing");

    try {
      // API 호출은 훅 내부에서 처리
      await api.checkout({
        items,
        couponCode,
        subtotal,
        discount,
        total,
        paymentInfo,
      });

      setStatus("success");
      setItems([]);
      setCouponCode("");
      return { success: true };
    } catch (error) {
      setStatus("error");
      return { success: false, error: error.message };
    }
  };

  return {
    items,
    setItems,
    couponCode,
    applyCoupon,
    subtotal,
    discount,
    total,
    status,
    processCheckout,
  };
};

// 순수 비즈니스 로직 테스트
test("calculateDiscount applies correct discount for SAVE10", () => {
  const items = [{ id: 1, name: "Item 1", price: 100, quantity: 1 }];

  expect(calculateDiscount(items, "SAVE10")).toBe(10);
});

// 사이드 이펙트가 있는 훅 테스트
test("useCheckout processes checkout successfully", async () => {
  // API 메서드 모킹
  const mockCheckoutApi = jest.fn().mockResolvedValue({ orderId: "123" });
  jest.spyOn(api, "checkout").mockImplementation(mockCheckoutApi);

  const { result } = renderHook(() => useCheckout());

  // 아이템 및 쿠폰 설정
  act(() => {
    result.current.setItems([
      { id: 1, name: "Item 1", price: 100, quantity: 1 },
    ]);
    result.current.applyCoupon("SAVE10");
  });

  // 주문 처리
  await act(async () => {
    await result.current.processCheckout({ cardNumber: "4111111111111111" });
  });

  expect(mockCheckoutApi).toHaveBeenCalledWith(
    expect.objectContaining({
      items: expect.any(Array),
      couponCode: "SAVE10",
      subtotal: 100,
      discount: 10,
      total: 90,
    })
  );

  expect(result.current.status).toBe("success");
  expect(result.current.items).toEqual([]);
});
```

### 나쁜 예

```jsx
// 비즈니스 로직과 사이드 이펙트가 혼합된 컴포넌트
const CheckoutPage = () => {
  const [items, setItems] = useState([]);
  const [couponCode, setCouponCode] = useState("");
  const [status, setStatus] = useState("idle");
  const [error, setError] = useState(null);

  const subtotal = items.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );

  // 비즈니스 로직과 사이드 이펙트가 혼합됨
  const calculateDiscount = () => {
    let discount = 0;

    switch (couponCode) {
      case "SAVE10":
        discount = subtotal * 0.1;
        // 사이드 이펙트: 쿠폰 사용 트래킹
        trackCouponUsage("SAVE10");
        break;
      case "SAVE20":
        discount = subtotal * 0.2;
        trackCouponUsage("SAVE20");
        break;
      case "50OFF":
        discount = subtotal >= 100 ? 50 : 0;
        trackCouponUsage("50OFF");
        break;
    }

    return discount;
  };

  const discount = calculateDiscount();
  const total = subtotal - discount;

  const handleCouponChange = (e) => {
    setCouponCode(e.target.value);
  };

  const handleCheckout = async () => {
    setStatus("processing");
    setError(null);

    try {
      // API 호출
      await fetch("/api/checkout", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          items,
          couponCode,
          subtotal,
          discount,
          total,
        }),
      });

      setStatus("success");
      // localStorage 사이드 이펙트
      localStorage.setItem("lastOrder", JSON.stringify({ items, total }));
      // 장바구니 초기화
      setItems([]);
      setCouponCode("");

      // 주문 완료 후 다른 페이지로 이동
      window.location.href = "/order-confirmation";
    } catch (err) {
      setStatus("error");
      setError(err.message);
    }
  };

  // 렌더링 로직...
};

// 다양한 사이드 이펙트가 혼합되어 테스트하기 어려움
// - 트래킹 호출
// - API 호출
// - localStorage 사용
// - 페이지 이동
```

## 9. 이벤트 핸들러 테스트 용이성

**핵심 원칙**: 이벤트 핸들러 함수를 분리하고 테스트 가능하게 작성합니다.

### 좋은 예

```jsx
// 이벤트 핸들러 로직을 분리한 커스텀 훅
const useSearchForm = (onSearch) => {
  const [query, setQuery] = useState("");
  const [category, setCategory] = useState("all");
  const [error, setError] = useState(null);

  const handleQueryChange = (e) => {
    setQuery(e.target.value);
    if (error) setError(null);
  };

  const handleCategoryChange = (e) => {
    setCategory(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    if (!query.trim()) {
      setError("Please enter a search term");
      return;
    }

    onSearch({ query, category });
  };

  const handleClear = () => {
    setQuery("");
    setCategory("all");
    setError(null);
  };

  return {
    query,
    category,
    error,
    handleQueryChange,
    handleCategoryChange,
    handleSubmit,
    handleClear,
  };
};

// 컴포넌트
const SearchForm = ({ onSearch }) => {
  const {
    query,
    category,
    error,
    handleQueryChange,
    handleCategoryChange,
    handleSubmit,
    handleClear,
  } = useSearchForm(onSearch);

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="text"
          value={query}
          onChange={handleQueryChange}
          placeholder="Search..."
        />
        {error && <p className="error">{error}</p>}
      </div>

      <select value={category} onChange={handleCategoryChange}>
        <option value="all">All Categories</option>
        <option value="electronics">Electronics</option>
        <option value="books">Books</option>
        <option value="clothing">Clothing</option>
      </select>

      <button type="submit">Search</button>
      <button type="button" onClick={handleClear}>
        Clear
      </button>
    </form>
  );
};

// 테스트
test("useSearchForm validates empty query", () => {
  const mockSearch = jest.fn();
  const { result } = renderHook(() => useSearchForm(mockSearch));

  // 빈 검색어로 제출
  act(() => {
    const mockEvent = { preventDefault: jest.fn() };
    result.current.handleSubmit(mockEvent);
  });

  expect(result.current.error).toBe("Please enter a search term");
  expect(mockSearch).not.toHaveBeenCalled();
});

test("useSearchForm calls onSearch with query and category", () => {
  const mockSearch = jest.fn();
  const { result } = renderHook(() => useSearchForm(mockSearch));

  // 검색어 설정
  act(() => {
    result.current.handleQueryChange({ target: { value: "laptop" } });
    result.current.handleCategoryChange({ target: { value: "electronics" } });
  });

  // 폼 제출
  act(() => {
    const mockEvent = { preventDefault: jest.fn() };
    result.current.handleSubmit(mockEvent);
  });

  expect(mockSearch).toHaveBeenCalledWith({
    query: "laptop",
    category: "electronics",
  });
});
```

### 나쁜 예

```jsx
// 이벤트 핸들러와 UI가 밀접하게 결합된 컴포넌트
const SearchForm = ({ onSearch }) => {
  const [query, setQuery] = useState("");
  const [category, setCategory] = useState("all");
  const [error, setError] = useState(null);

  // 이벤트 핸들러가 컴포넌트 내부에 정의됨
  const handleSubmit = (e) => {
    e.preventDefault();

    // 유효성 검사 로직
    if (!query.trim()) {
      setError("Please enter a search term");
      return;
    }

    // 검색 실행
    onSearch({ query, category });

    // 상태 업데이트
    if (error) setError(null);

    // 분석 이벤트 추적
    trackSearchEvent(query, category);
  };

  // 컴포넌트 렌더링...

  return <form onSubmit={handleSubmit}>{/* 폼 요소들 */}</form>;
};

// 이벤트 핸들러가 컴포넌트와 밀접하게 결합되어 있어 테스트하기 어려움
```

## 10. Props 및 상태 검증

**핵심 원칙**: 컴포넌트의 props와 상태를 명확하게 정의하고 검증합니다.

### 좋은 예

```jsx
import PropTypes from "prop-types";

// 타입 정의 (TypeScript)
interface UserCardProps {
  user: {
    id: string,
    name: string,
    email: string,
    role: string,
    avatar?: string,
  };
  onEdit?: (userId: string) => void;
  onDelete?: (userId: string) => void;
  isActive: boolean;
}

// 기본값 설정
const defaultProps = {
  onEdit: undefined,
  onDelete: undefined,
};

// 컴포넌트
const UserCard: React.FC<UserCardProps> = ({
  user,
  onEdit,
  onDelete,
  isActive,
}) => {
  // 조기 오류 검사
  if (!user) {
    console.error("UserCard: user prop is required");
    return <div>Error: User data missing</div>;
  }

  return (
    <div className={`user-card ${isActive ? "active" : "inactive"}`}>
      {user.avatar ? (
        <img src={user.avatar} alt={`${user.name}'s avatar`} />
      ) : (
        <div className="avatar-placeholder">{user.name.charAt(0)}</div>
      )}

      <div className="user-info">
        <h3>{user.name}</h3>
        <p>{user.email}</p>
        <p>Role: {user.role}</p>
      </div>

      <div className="user-actions">
        {onEdit && <button onClick={() => onEdit(user.id)}>Edit</button>}
        {onDelete && <button onClick={() => onDelete(user.id)}>Delete</button>}
      </div>
    </div>
  );
};

// PropTypes (JavaScript)
UserCard.propTypes = {
  user: PropTypes.shape({
    id: PropTypes.string.isRequired,
    name: PropTypes.string.isRequired,
    email: PropTypes.string.isRequired,
    role: PropTypes.string.isRequired,
    avatar: PropTypes.string,
  }).isRequired,
  onEdit: PropTypes.func,
  onDelete: PropTypes.func,
  isActive: PropTypes.bool.isRequired,
};

UserCard.defaultProps = defaultProps;

// 테스트
test("UserCard renders user information correctly", () => {
  const user = {
    id: "123",
    name: "John Doe",
    email: "john@example.com",
    role: "Admin",
  };

  const { getByText } = render(<UserCard user={user} isActive={true} />);

  expect(getByText("John Doe")).toBeInTheDocument();
  expect(getByText("john@example.com")).toBeInTheDocument();
  expect(getByText("Role: Admin")).toBeInTheDocument();
});

test("UserCard calls onEdit when edit button clicked", () => {
  const user = {
    id: "123",
    name: "John Doe",
    email: "john@example.com",
    role: "Admin",
  };

  const handleEdit = jest.fn();

  const { getByText } = render(
    <UserCard user={user} isActive={true} onEdit={handleEdit} />
  );

  fireEvent.click(getByText("Edit"));
  expect(handleEdit).toHaveBeenCalledWith("123");
});
```

### 나쁜 예

```jsx
// props 검증이 없는 컴포넌트
const UserCard = (props) => {
  return (
    <div className={`user-card ${props.isActive ? "active" : "inactive"}`}>
      <img src={props.user.avatar} alt="User avatar" />

      <div className="user-info">
        <h3>{props.user.name}</h3>
        <p>{props.user.email}</p>
        <p>Role: {props.user.role}</p>
      </div>

      <div className="user-actions">
        <button onClick={() => props.onEdit(props.user.id)}>Edit</button>
        <button onClick={() => props.onDelete(props.user.id)}>Delete</button>
      </div>
    </div>
  );
};

// 문제점:
// - props 타입이 정의되지 않음
// - 필수 props 검증 없음
// - 기본값 설정 없음
// - props.user가 없거나 속성이 누락된 경우 오류 발생
// - props.onEdit, props.onDelete가 함수가 아닌 경우 오류 발생
```

## 11. 접근성 및 UI 컴포넌트 테스트

**핵심 원칙**: UI 컴포넌트의 접근성과 동작을 테스트합니다.

### 좋은 예

```jsx
// 접근성 고려한 컴포넌트
const Accordion = ({ items }) => {
  const [openIndex, setOpenIndex] = useState(null);

  const toggleItem = (index) => {
    setOpenIndex(openIndex === index ? null : index);
  };

  return (
    <div className="accordion">
      {items.map((item, index) => (
        <div
          key={index}
          className="accordion-item"
          data-testid={`accordion-item-${index}`}
        >
          <button
            className="accordion-header"
            onClick={() => toggleItem(index)}
            aria-expanded={openIndex === index}
            aria-controls={`accordion-content-${index}`}
          >
            <span>{item.title}</span>
            <span className="accordion-icon">
              {openIndex === index ? "▲" : "▼"}
            </span>
          </button>

          <div
            id={`accordion-content-${index}`}
            className="accordion-content"
            hidden={openIndex !== index}
          >
            {item.content}
          </div>
        </div>
      ))}
    </div>
  );
};

// 테스트
describe("Accordion", () => {
  const mockItems = [
    { title: "Section 1", content: "Content for section 1" },
    { title: "Section 2", content: "Content for section 2" },
  ];

  test("renders all accordion items", () => {
    const { getAllByRole } = render(<Accordion items={mockItems} />);
    const buttons = getAllByRole("button");

    expect(buttons).toHaveLength(2);
    expect(buttons[0]).toHaveTextContent("Section 1");
    expect(buttons[1]).toHaveTextContent("Section 2");
  });

  test("contents are hidden by default", () => {
    const { getByText } = render(<Accordion items={mockItems} />);

    const content1 = getByText("Content for section 1");
    const content2 = getByText("Content for section 2");

    expect(content1.parentElement).toHaveAttribute("hidden");
    expect(content2.parentElement).toHaveAttribute("hidden");
  });

  test("clicking header shows the content", () => {
    const { getAllByRole, getByText } = render(<Accordion items={mockItems} />);
    const buttons = getAllByRole("button");

    // 첫 번째 아이템 클릭
    fireEvent.click(buttons[0]);

    const content1 = getByText("Content for section 1");
    const content2 = getByText("Content for section 2");

    // 첫 번째 콘텐츠는 표시되고, 두 번째는 숨겨져 있어야 함
    expect(content1.parentElement).not.toHaveAttribute("hidden");
    expect(content2.parentElement).toHaveAttribute("hidden");

    // aria-expanded 속성 확인
    expect(buttons[0]).toHaveAttribute("aria-expanded", "true");
    expect(buttons[1]).toHaveAttribute("aria-expanded", "false");
  });

  test("clicking an open section closes it", () => {
    const { getAllByRole, getByText } = render(<Accordion items={mockItems} />);
    const buttons = getAllByRole("button");

    // 첫 번째 아이템 열기
    fireEvent.click(buttons[0]);

    // 같은 아이템 다시 클릭
    fireEvent.click(buttons[0]);

    const content1 = getByText("Content for section 1");

    // 콘텐츠가 다시 숨겨져야 함
    expect(content1.parentElement).toHaveAttribute("hidden");
    expect(buttons[0]).toHaveAttribute("aria-expanded", "false");
  });
});
```

### 나쁜 예

```jsx
// 접근성을 고려하지 않은 컴포넌트
const Accordion = ({ items }) => {
  const [openIndex, setOpenIndex] = useState(null);

  return (
    <div className="accordion">
      {items.map((item, index) => (
        <div key={index} className="accordion-item">
          <div
            className="accordion-header"
            onClick={() => setOpenIndex(openIndex === index ? null : index)}
          >
            {item.title}
            {openIndex === index ? "▲" : "▼"}
          </div>

          <div
            className="accordion-content"
            style={{ display: openIndex === index ? "block" : "none" }}
          >
            {item.content}
          </div>
        </div>
      ))}
    </div>
  );
};

// 문제점:
// - div를 클릭 요소로 사용 (button이 더 적합)
// - 키보드 접근성 없음
// - ARIA 속성 없음 (aria-expanded, aria-controls)
// - 테스트 ID 또는 접근성 속성 없음
// - hidden 속성 대신 CSS display 속성을 사용
```

## 결론

프론트엔드 코드를 테스트하기 좋게 작성하는 것은 단순히 테스트를 쉽게 만드는 것을 넘어 더 나은 코드 구조와 유지보수성을 제공합니다. 이 문서에서 다룬 원칙들을 적용하면 테스트가 용이한 코드를 작성할 수 있으며, 이는 궁극적으로 더 안정적이고 유지보수가 쉬운 프론트엔드 애플리케이션으로 이어집니다.

기억해야 할 핵심 내용:

1. 컴포넌트와 함수의 책임을 명확히 분리
2. 의존성 주입을 통한 테스트 용이성 확보
3. 순수 함수와 사이드 이펙트 격리
4. 작은 단위의 컴포넌트와 함수 작성
5. 커스텀 훅으로 로직 분리
6. 예측 가능한 상태 관리
7. 비동기 코드와 이벤트 핸들러 모듈화
8. Props 및 상태 타입 검증
9. 접근성과 UI 컴포넌트 테스트 고려

이러한 원칙들을 일관되게 적용하면, 단위 테스트뿐만 아니라 통합 테스트와 E2E 테스트까지 효과적으로 작성할 수 있는 기반이 마련됩니다.
