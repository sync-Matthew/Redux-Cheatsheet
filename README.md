# Redux-Cheatsheet.

<h2>Setup Redux</h2>  

1. Create a directory outside of app and name it `redux`.  
2. Create a subdirectory called `reducers`.  
3. Create a reducer within the subdirectory. In this example we'll be creating the reducer responsible for holding all the user's non-sensitive information, `userSlice.js`.  

```JavaScript
// userSlice.js

"use client"

import { createSlice } from '@reduxjs/toolkit'

const initialState = {
    email: "", // user email
    first_name: "", // user first name
    last_name: "", // user last name
    realms: [], // list of realms
    logged_in: false, // is the user logged in?
}

export const userSlice = createSlice({
  name: 'user',
  initialState: initialState,
  reducers: {

    setUser: (state, action) => {
        state.email = action.payload.email;
        state.first_name = action.payload.first_name;
        state.last_name = action.payload.last_name;
        state.realms = action.payload.realms;
        state.logged_in = action.payload.logged_in;
    },

    setUserEmail: (state, action) => {
      state.email = action.payload;
    },

    resetUserState: () => initialState,

  }
})

// Action creators are generated for each case reducer function
export const { setUser, resetUserState, setUserEmail, loginUser } = userSlice.actions

export const selectUser = (state) => state.user

export default userSlice.reducer
```  

4. Create `store.js` inside the redux directory.

```JavaScript
// store.js

"use client"

import { configureStore } from '@reduxjs/toolkit'
import userReducer from './reducers/userSlice'

export const store = configureStore({
  reducer: {
    user: userReducer,
  },
})
```  

5. Create `provider.js` inside the redux directory.

```JavaScript
// provider.js

"use client";

import { store } from "./store";
import { Provider } from "react-redux";

export function Providers({ children }) {
  return <Provider store={store}>{children}</Provider>;
}
```  

6. Wrap your RootLayout with your Provider.

```JavaScript
// app/layout.js

import './globals.css'
import { Inter } from 'next/font/google'
import { Providers } from "../redux/provider";
const inter = Inter({ subsets: ['latin'] })

export const metadata = {
  title: 'Buildaible',
  description: 'Buildaible',
}

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <Providers>
          <div className="flex flex-col h-full relative">
            {children}
          </div>
        </Providers>
      </body>
    </html>
  )
}
```  

<h2>Retrieve Data from Store</h2>  

1. Import `useSelector`  
2. Add `use client` to the top of your component.  
3. Import your action from the reducer. In this example we import `selectUser`  
4. Create a variable to hold the retrieved data and call your action with `useSelector`.  

![image](https://github.com/sync-Matthew/Redux-Cheatsheet./assets/109091963/57d20de4-f359-4943-9d14-961cff16e8f5)

