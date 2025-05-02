# My-project
Interactive dashboard for monthly sales data to improve KPI visibility and insights
import React from 'react';
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { PieChart, Pie, Cell, Legend, ResponsiveContainer, Tooltip } from 'recharts';
import { SalesByCategory } from '@/services/salesData';

interface CategoryChartProps {
  data: SalesByCategory[];
}

const COLORS = ['#2D7DD2', '#45B7D1', '#26C485', '#F58A07', '#F45B69'];

const formatCurrency = (value: number) => {
  return `$${value.toLocaleString()}`;
};

const CategoryChart = ({ data }: CategoryChartProps) => {
  return (
    <Card>
      <CardHeader>
        <CardTitle>Sales by Category</CardTitle>
      </CardHeader>
      <CardContent className="h-[300px]">
        <ResponsiveContainer width="100%" height="100%">
          <PieChart>
            <Pie
              data={data}
              cx="50%"
              cy="50%"
              labelLine={false}
              outerRadius={80}
              fill="#8884d8"
              dataKey="sales"
              nameKey="category"
              label={({ name, percent }) => `${name}: ${(percent * 100).toFixed(0)}%`}
            >
              {data.map((entry, index) => (
                <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
              ))}
            </Pie>
            <Legend />
            <Tooltip formatter={(value) => formatCurrency(Number(value))} />
          </PieChart>
        </ResponsiveContainer>
      </CardContent>
    </Card>
  );
};

export default CategoryChart;
